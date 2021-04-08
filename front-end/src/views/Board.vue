<template>
<div class="bulletin_board">
    <p v-if="this.$root.$data.user"><a href="/add"><button>Create an announcement/event</button></a></p>
    <p v-else>You must be signed in to create announcements or events.</p>
    <div v-for="event in events" :key="event.date">
        <div class="announcement" :id="event._id">
            <h3>{{event.title}}</h3>
            <p>{{event.user}} created this announcement/event</p>
            <p>What? - {{event.description}}</p>
            <p>When? - {{event.date}}</p>
            <p>Where? - {{event.location}}</p>
            <p>How many people are attending? - <span v-if="event.attendees.length > 0">{{event.attendees.length}}</span><span v-else>Looks like nobody yet</span></p>
            <div>Which of your friends are attending? - 
                <span v-if="event.attendees.length > 0">
                    <ul>
                        <li v-for="attendee in event.attendees" :key="attendee">{{attendee}}</li>
                    </ul>
                </span>
                <span v-else>So far, none of them</span>
            </div>
            <span v-if="$root.$data.user != null">
                <div class="row" v-if="$root.$data.user._id == event.user_id">
                    <button type="button" v-on:click="edit(event)">Edit</button>
                    <button v-on:click="remove(event)" type='button'>Delete</button>
                </div>
            </span>
            <div :id="event.date+event._id" hidden>
                    <form v-on:submit.prevent="submitEdit(event)">
                    <div class="row"><input required type='text' placeholder="New Title" v-model='title'></div>
                    <div class='row'><input required type='text' v-model='description' placeholder="New Description/About Info"></div>
                        <div class='row'>New Date - <input required type='date' v-model='date'></div>
                        <div class='row'><input required type='text' v-model='location' placeholder="New Location"></div>
                        <div class="row"><button :name="event._id" type="submit" >Submit Changes</button>
                    </div>
                    </form>
                    </div>
            <div class="row" v-if="$root.$data.user">
                <form>
                    <button class="attendButton" v-on:click="willAttend(event)" type="button">Press to RSVP</button>
                </form>
                <div v-if="errorRSVP">{{errorRSVP}}</div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios';
    
export default {
  name: "Board",
    data() {
        return {events: this.events};
    },
    created() {
        this.getEvents();
    },
        methods: {
            async getEvents() {
                try {
                    let res = await axios.get("/api/events");
                    this.events = res.data;
                    return true;
                } catch (error) {
                    console.log(error);
                }
            },
            async willAttend(event) {
                let fullname = this.$root.$data.user.firstName + " " + this.$root.$data.user.lastName;
                
                // check to make sure it isn't there yet
                let response = await axios.get("/api/events/" + event._id);
                
                let found = false;
                console.log(response);
                for (let i = 0; i < response.data.attendees.length; i++) {
                    if (response.data.attendees[i] == fullname) {
                        found = true;
                    }
                }
                
                if (!found) {
                    try {                
                        await axios.put("/api/events/attendee/" + event._id, {
                            new_attendee: 1,
                            new_attendee_name: fullname
                        });
                        this.getEvents();
                        return true;
                    } catch (error) {
                        console.log(error);
                    }
                } else {
                    this.errorRSVP = "You have already RSVPed";
                    this.getEvents();
                    return true;
                }
            },
            edit(event) {
                document.getElementById(event.date + event._id).hidden = false;
            },
            async submitEdit(event) {
                console.log("in submitEdit()");
                try {
                    await axios.put("/api/events/" + event._id, {
                        title: this.title,
                        description: this.description,
                        location: this.location,
                        date: this.date
                    });
                    this.getEvents();
                    return true;
                } catch (error) {
                    console.log(error);
                }
            },
            async remove(event) {
                try {
                    await axios.delete("/api/events/" + event._id);
                    this.getEvents();
                    return true;
                } catch(error) {
                    console.log(error);
                }
            }
        }
};
</script>