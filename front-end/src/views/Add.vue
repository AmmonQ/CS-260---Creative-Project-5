<template>
<div class="add">
    <h3>Add a new announcement/event</h3>
    <form id="newEventForm" v-on:submit.prevent="add()">
        <div class="row">
            <label for="title">Title: <input required v-model="title" type="text" name="title" class="pure-input-rounded"></label>
        </div>
        <div class="row">
            <label for="description">Announcement/Event Description: <input required v-model="description" type="text" name="description"></label>
        </div>
        <div class="row">
            <label for="location">Where will this take place? <input required v-model="location" type="text" name="location"></label>
        </div>
        <div class="row">
            <label for="date">When will this take place? <input required v-model="date" type="date" name="date"></label>
        </div>
        <div class="row">
            <button class="button-primary" type="submit">Add Annoucement/Event</button>
        </div>
    </form>
</div>
</template>

<script>
import axios from 'axios';
    
export default {
    name: "Add",
    data() {
        return {
            title: "",
            description: "",
            location: "",
            date: "",
        }
    },
    methods: {
        async add() {
            try {
                await axios.post("/api/events", {
                    title: this.title,
                    description: this.description,
                    location: this.location,
                    date: this.date,
                    user: this.$root.$data.user.firstName + " " + this.$root.$data.user.lastName,
                    user_id: this.$root.$data.user._id
                });
                
                this.$router.push('Board');
            } catch (error) {
                console.log(error);
            }
        }
    }
}
</script>