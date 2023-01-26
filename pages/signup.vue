<template>
    <div class="text-center">
      <form class="form-signin" @submit.prevent="userRegister">
        <h1 class="h3 mb-3 mt-3 font-weight-normal">Enter user name and password</h1>
        <label for="inputUsername" class="sr-only">User name</label>
        <input id="inputUsername" class="form-control" placeholder="User name" required="" v-model="register.username">
        <label for="inputPassword" class="sr-only">Password</label>
        <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Пароль" required="" v-model="register.password">
        <label for="ReInputPassword" class="sr-only">Repeat password</label>
        <input type="password" id="ReInputPassword" class="form-control mt-2" placeholder="Repeat password" required="" v-model="register.password_verifier">
        <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Sign up</button>
      </form>
    </div>
  </template>
  
<script>
    export default {
        layout: "post_detail",
        data() {
            return {
                register: {
                    username: '',
                    password: '',
                    password_verifier: '',
                },
            }
        },
    methods: {
        async userRegister() {
            try {
                let response = await this.$axios.post('/api/register/', {
                    username: this.register.username,
                    password: this.register.password,
                    password_verifier: this.register.password_verifier
                })
                console.log(response)
                await this.$auth.loginWith('local', {
                    data: {
                        username: this.register.username,
                        password: this.register.password
                    },
                })
                this.$router.push('/')
            } catch (err) {
                console.log(err)
            }
        }
    }
  }
  </script>