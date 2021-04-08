<template>
  <div id="app">
      <div class="login_p" v-if="this.$root.$data.user">
        Welcome, {{user.firstName}} {{user.lastName}}! <a @click="logout()"><button>Logout</button></a>
        </div>
         <router-link class="login_p" to="/login" v-else id="login_button"><button>Login or Register</button></router-link>
    <div id="nav">
        
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
        <router-link to="/board">Bulletin Board</router-link>
    </div>
    
    <router-view />
  </div>
</template>

<script>
import axios from 'axios';

export default {
    async created() {
        try {
          let response = await axios.get('/api/users');
          this.$root.$data.user = response.data.user;
        } catch (error) {
          this.$root.$data.user = null;
        }
    },
    computed: {
        user() {
            return this.$root.$data.user;
        }
    },
    methods: {
        async logout() {
            try {
                await axios.delete("/api/users");
                this.$root.$data.user = null;
            } catch (error) {
                this.$root.$data.user = null;
            }
        }
    }
}
</script>

<style>
#app {
  text-align: center;
    margin-top: 4em;
}
    
    .login_p {
        margin-bottom: 1em;
    }
</style>
