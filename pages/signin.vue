<template>
    <div>
      <div class="text-center">
        <form class="form-signin" @submit.prevent="userLogin">
          <h1 class="h3 mb-3 mt-3 font-weight-normal">Please enter login and password</h1>
          <label for="inputUsername" class="sr-only">User name</label>
          <input id="inputUsername" class="form-control" placeholder="User name" required="" v-model="login.username">
          <label for="inputPassword" class="sr-only">Password</label>
          <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Password" required="" v-model="login.password">
          <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Sign in</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    layout: "post_detail",
    data() {
      return {
        login: {
          username: '',
          password: ''
        },
      }
    },
    methods: {
      async userLogin() {
        try {
          let response = await this.$auth.loginWith('local', { data: this.login })
          console.log(response)
          this.$router.back()
          // this.$router.push('/')
        } catch (err) {
          console.log(err)
        }
      }
    }
  }
  </script>