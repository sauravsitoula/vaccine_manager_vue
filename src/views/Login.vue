<template>
  <main class="form-signin">
    <form @submit.prevent="submit">
      <h1 class="h3 mb-3 fw-normal">Please sign in</h1>

      <div class="form-floating">
        <input type="email" class="form-control" name="email" placeholder="name@example.com">
        <label>Email</label>
      </div>

      <div class="form-floating">
        <input type="password" class="form-control" name="password" placeholder="Password">
        <label>Password</label>
      </div>

      <button class="w-100 btn btn-lg btn-primary" type="submit">Submit</button>
    </form>
  </main>
</template>

<script>
import {useRouter} from "vue-router";
import axios from "axios";

export default {
  name: "Login",
  setup() {
    const router = useRouter();

    const submit =  e => {
      const form = new FormData(e.target);

      const inputs = Object.fromEntries(form.entries());

       axios.post('signin', inputs, {
        withCredentials: true
      }).then(function(response){
        console.log(response)
        localStorage.setItem('authToken', `Bearer ${response.data.accessToken}`)
        axios.defaults.headers.common['Authorization'] = localStorage.authToken;
        router.push('/');
        })
    }

    return {
      submit
    }
  },
  mounted() {
    this.$emit('inLogin');
  },
  methods: {

  }
}
</script>
