<template>
<div class="announcement">
    <h1 class="title">{{title}}</h1>
    <p class="information">{{information}}</p>
    <address class="location">This event will be held at {{location}} on <span class="when">{{date}}</span></address>
    <button v-on:click="edit()">Edit</button>
    <button v-on:click="delete()">Delete</button>
    <button id="attendButton" class="button_off" v-on:click="attendToggle()">Not Attending - Press to Attend</button>
</div>
</template>

<style scoped>
    .announcement {
        border: 4px red dashed;
    }
</style>

<script>
export default {
    name: "Announcement",
    methods() {
        attendToggle() {
            if (document.getElementById("attendButton").className === "button_off") {
                document.getElementById("attendButton").classList.remove("button_off");
                document.getElementById("attendButton").classList.add("button_on");
                document.getElementById("attendButton").textContent = "Not Attending - Press to Attend";
            } else {
                document.getElementById("attendButton").classList.remove("button_on");
                document.getElementById("attendButton").classList.add("button_off");
                document.getElementById("attendButton").textContent = "Attending!";
            }
        }
    }
}
</script>

<style scoped>
    /* not attending */
    .button_off {
        background-color: purple;
    }
    
    /* attending */
    .button_on {
        background-color: blue;
    }
</style>