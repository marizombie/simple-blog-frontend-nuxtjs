<template>
    <div>
      <Header :h1="title"/>
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <nav aria-label="breadcrumb" class="mt-4">
            <ol class="breadcrumb">
              <li class="breadcrumb-item"><nuxt-link to="/">Home</nuxt-link></li>
              <li class="breadcrumb-item active" aria-current="page">Contacts</li>
            </ol>
          </nav>
          <p class="lead">Your message has been sent</p>
        </div>
      </div>
    </div>
    </div>
  </template>
  
  <script>
  export default {
    layout: "post_detail",
    name: "success",
    data() {
      return {
        title: "Thank you for contacting me and have a good day"
      }
    }
  }
  </script>