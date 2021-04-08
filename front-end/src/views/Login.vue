<template>
<div class="login">
    <form v-on:submit.prevent="register()">
        <div class="row">
            <legend>Register</legend>
        </div>
        <div class="row">
            <input type="text" placeholder="First name here" v-model="firstName">
            <input type="text" placeholder="Last name here" v-model="lastName">
            </div>
        <div class="row">
            <input type="text" placeholder="Username here" v-model="username">
            <input type="password" placeholder="Password here" v-model="password">
        </div>
        <div class="row">
            <button type="submit" class="button-primary" >Register</button>
        </div>
    </form>
    <p v-if="error">{{error}}</p>
    
    <form v-on:submit.prevent="login()">
        <div class="row">
            <legend>Login</legend>
        </div>
        <div class="row">
            <input type="text" placeholder="Username here" v-model="usernameLogin">
            <input placeholder="Password here" type="password" v-model="passwordLogin">
        </div>
        <div class="row">
            <button type="submit" class="button-primary">Login</button>
        </div>
    </form>
    
    <p v-if="errorLogin">{{errorLogin}}</p>
</div>
</template>

<script>
import axios from 'axios';
    
export default {
    name: "Login",
    data() {
        return {
            firstName: '',
            lastName: '',
            username: '',
            password: '',
            usernameLogin: '',
            passwordLogin: '',
            error: '',
            errorLogin: ''
        }
    },
    methods: {
        async register() {
          this.error = '';
          this.errorLogin = '';
          if (!this.firstName || !this.lastName || !this.username || !this.password) {
              return;
          }
          try {
            let response = await axios.post('/api/users', {
              firstName: this.firstName,
              lastName: this.lastName,
              username: this.username,
              password: this.password,
            });
            this.$root.$data.user = response.data.user;
          } catch (error) {
            this.error = error.response.data.message;
            this.$root.$data.user = null;
          }
            this.$router.push({ name: "Home" });
        },
        async login() {
            this.error = '';
            this.errorLogin = '';
             if (!this.usernameLogin || !this.passwordLogin) {
                return;
             }
              try {
                let response = await axios.post('/api/users/login', {
                  username: this.usernameLogin,
                  password: this.passwordLogin,
                });
                this.$root.$data.user = response.data.user;
              } catch (error) {
                this.errorLogin = "Error: " + error.response.data.message;
                this.$root.$data.user = null;
              }
            
            this.$router.push({ name: "Home" });
        }
    }
}
</script>