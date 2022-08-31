<template>
<div id="main-wrapper">
  <header class="p-3 bg-dark text-white">
    <div class="container">
      <div class="d-flex flex-wrap align-items-center justify-content-center justify-content-lg-start">
        <div class="unAuthenticated-nav">
          <router-link to="/login" class="btn btn-outline-light me-2" v-show="isInRegister">Login</router-link>
          <router-link to="/register" class="btn btn-outline-light me-2" v-show="isInLogin">Register</router-link>
          
        </div>
        <div class="authenticated-nav" v-show="isAuthenticated">
          <router-link to="/" class="btn btn-outline-light me-2">Home</router-link>
          <button @click="logout">Logout</button>
        </div>
      </div>
    </div>
  </header>
  <div class="body-wrapper">
    <!-- <router-view :toUpdateVaccine="" ></router-view>/> -->
    <router-view :toUpdateVaccine="this.data" @inLogin="loginPage" @inRegister="registerPage" @loggedIn="authenticatedPage"></router-view>
  </div>
</div>
</template>

<style>
.body-wrapper {
  padding: 0px 20px;
}
.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}

.form-signin .checkbox {
  font-weight: 400;
}

.form-signin .form-floating:focus-within {
  z-index: 2;
}

.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}

.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}

.unAuthenticated-nav {
  margin-left: 80%;
}

</style>

<script>
  import axios from "axios";
import router from "./router";
  export default {
    data() {
      return {
        isAuthenticated: false,
        isInLogin: false,
        isInRegister: false
      }
    },
    methods: {
      logout() {
        axios.defaults.headers.common['Authorization'] = localStorage.getItem('authToken');
        axios.delete('signout').then(function(res) {
          router.push('/login');
          localStorage.setItem('authToken', '')
        });
      },
      loginPage() {
        this.isInLogin=true;
        this.isInRegister=false;
        this.isAuthenticated=false;
      },
      registerPage(){
        this.isInLogin=false;
        this.isInRegister=true;
        this.isAuthenticated=false;
      },
      authenticatedPage() {
        console.log('AUTHENTICATED PAGEEEEEEEEE')
        this.isInLogin=false;
        this.isInRegister=false;
        this.isAuthenticated=true;
      }
    },
    
  }
</script>
