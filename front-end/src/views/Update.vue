<template>
<div class="update">
    <form id="updateEventForm">
        <div class="row">
            <label for="title">Title: <input v-model="title" type="text" name="title" value="{{title}}" class="pure-input-rounded"></label>
        </div>
        <div class="row">
            <label for="description">Announcement/Event Description: <input v-model="description" type="text" name="description"></label>
        </div>
        <div class="row">
            <label for="location">Where will this take place? <input v-model="location" type="text" name="location"></label>
        </div>
        <div class="row">
            <label for="date">When will this take place? <input v-model="date" type="date" name="date"></label>
        </div>
    </form>
</div>
</template>

<script>
import axios from 'axios';
    
export default {
    name: "Update",
    data() {
        return {
            axios.get("/api/events/:id");
        }
    },
    methods: {
        async update() {
            
        }
    }
}
</script>