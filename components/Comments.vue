<template>
  <div>
    <div class="card my-4">
      <h5 class="card-header">Leave a comment:</h5>
      <div class="card-body" v-if="loggedIn">
        <form @submit.prevent="addComment">
          <div class="form-group">
            <textarea class="form-control" rows="3" v-model="new_comment"></textarea>
          </div>
          <button type="submit" class="btn btn-primary" :disabled="!isComplete">Send</button>
        </form>
      </div>
      <div v-else>
        <h6 class="card-header"><nuxt-link to="/signin">Sign in</nuxt-link> or <nuxt-link to="/signup">Sign up</nuxt-link> to leave a comment </h6>
      </div>
    </div>
    <div class="media mb-4" v-for="comment in comments" :key="comment.id">
      <img class="d-flex mr-3 rounded-circle" src="http://placehold.it/50x50" alt="">
      <div class="media-body">
        <h5 class="mt-0">{{comment.username}}</h5>
        {{comment.text}}
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";
  export default {
    props: ['comments', 'post'],
    data() {
      return {
        new_comment: '',
      }
    },
    methods: {
      async addComment() {

        let commentData = new FormData();
        commentData.set('text', this.new_comment);
        commentData.set('slug', this.$props.post.slug);
        commentData.set('username', this.user.username);
        console.log(this.user.username);
        console.log('submitting data...');
        axios({
            method: 'post',
            url: 'http://localhost:8000/api/comments/',
            data: commentData
        }).then(function (response) {
            console.log(response);
            this.new_comment = '';
            this.comments.splice(0, 0, response.data);
        }).catch(function (error) {
            console.log(error.response.data);
        });

        // try {
        //   let response = await this.$axios.post('http://127.0.0.1:8000/api/comments/', {
        //   post: this.$props.post.slug,
        //   username: this.user.username,
        //   text: this.new_comment,
        // })
        //   this.new_comment = '';
        //   this.comments.splice(0, 0, response.data)
        // console.log(response)
        // } catch (error) {
        //   console.log(error.response.data)
        // }
      },
    },
    computed: {
      loggedIn() {
        return this.$auth.loggedIn
      },
      user() {
        return this.$auth.user
      },
      isComplete () {
        return this.new_comment;
      },
    }
  }
</script>