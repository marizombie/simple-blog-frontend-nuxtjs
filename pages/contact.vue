<template>
    <div>
    <Header :h1="title"/>
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <nav aria-label="breadcrumb" class="mt-4">
            <ol class="breadcrumb">
              <li class="breadcrumb-item"><a href="">Home</a></li>
              <li class="breadcrumb-item active" aria-current="page">Contacts</li>
            </ol>
          </nav>
          <p class="lead">For contact fill the form</p>
          <form name="contact-form">
            <div class="row">
              <div class="col-md-6" v-bind:class="{ 'fld-error': $v.form.name.$error }">
                <div class="md-form mb-0">
                  <label for="name" class="sr-only">Name</label>
                  <input type="text" id="name" class="form-control" placeholder="Name" v-model="form.name"
                  @input="$v.form.name.$touch()">
                </div>
                <span class="msg-error" v-if="!$v.form.name.required">
                  <small>Field must be non empty</small>
                </span>
                <span class="msg-error" v-if="!$v.form.name.minLength">
                  <small>Name must be no less than {{ $v.form.name.$params.minLength.min }} symbols</small>
                </span>
              </div>
              
              <div class="col-md-6" v-bind:class="{ 'fld-error': $v.form.email.$error }">
                <div class="md-form mb-0">
                  <label for="email" class="sr-only">Email</label>
                  <input type="text" id="email" class="form-control" placeholder="Email" v-model="form.email"
                  @input="$v.form.email.$touch()">
                </div>
                <span class="msg-error" v-if="!$v.form.email.required">
                  <small>Field must be non empty</small>
                </span>
                <span class="msg-error" v-if="!$v.form.email.email">
                  <small>Doesn't look like email, please check it</small>
                </span>
              </div>
            </div>

            <div class="row mt-3">
              <div class="col-md-12" v-bind:class="{ 'fld-error': $v.form.subject.$error }">
                <div class="md-form mb-0">
                  <label for="subject" class="sr-only">Subject</label>
                  <input type="text" id="subject" class="form-control" placeholder="Subject" v-model="form.subject"
                  @input="$v.form.subject.$touch()">
                </div>
                <span class="msg-error" v-if="!$v.form.subject.required">
                  <small>Field must be non empty</small>
                </span>
                <span class="msg-error" v-if="!$v.form.subject.minLength">
                  <small>Subject must be no less than {{ $v.form.subject.$params.minLength.min }} symbols</small>
                </span>
              </div>
            </div>

            <div class="row mt-3">
              <div class="col-md-12" v-bind:class="{ 'fld-error': $v.form.message.$error }">
                <div class="md-form">
                  <label for="message" class="sr-only">Your message</label>
                  <textarea type="text" id="message" rows="2" class="form-control md-textarea" 
                  placeholder="Your message" v-model="form.message" @input="$v.form.message.$touch()"></textarea>
                </div>
                <span class="msg-error" v-if="!$v.form.message.required">
                  <small>Field must be non empty</small>
                </span>
                <span class="msg-error" v-if="!$v.form.message.minLength">
                  <small>Message must be no less than {{ $v.form.message.$params.minLength.min }} symbols</small>
                </span>
              </div>
            </div>
          </form>
          <div class="text-center text-md-left mt-3">
            <button class="btn btn-primary" type="submit" @click.prevent="submitForm"
            :disabled="!isComplete">Send</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { required, minLength, email } from 'vuelidate/lib/validators'

export default {
  layout: "post_detail",
  data() {
    return {
        title: "Contacts",
        form: {
            name: '',
            email: '',
            subject: '',
            message: '',
        }
      }
    },
  methods: {
    submitForm() {
        let contactFormData = new FormData();
        contactFormData.set('name', this.form.name);
        contactFormData.set('email', this.form.email);
        contactFormData.set('subject', this.form.subject);
        contactFormData.set('message', this.form.message);
        console.log('submitting data...');
        axios({
            method: 'post',
            url: 'http://localhost:8000/api/feedback/',
            data: contactFormData
        }).then(function (response) {
            console.log(response);
        }).catch(function (error) {
            console.log(error);
        });
        this.$router.push("/success");
    }
  },
  computed: {
      isComplete () {
      return this.form.name && this.form.email && this.form.subject && this.form.message;
      }
  },
  validations: {
    form: {
      name: {
        required,
        minLength: minLength( 2 )
      },
      email: {
        email,
        required
      },
      subject: {
        required,
        minLength: minLength( 10 )
      },
      message: {
        required,
        minLength: minLength( 10 )
      }
    }
  },
}
</script>

<style type="text/css">
.fld-error .msg-error  {
  display: block;
  color: #dc3545;
}
.msg-error {
  display: none;
}
</style>