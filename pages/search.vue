<template>
    <div>
      <!-- <Search @searchPosts="posts = $event"/> -->
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <nav aria-label="breadcrumb" class="mt-4">
              <ol class="breadcrumb">
                <li class="breadcrumb-item"><nuxt-link to="/">Home</nuxt-link></li>
                <li class="breadcrumb-item active" aria-current="page">Search</li>
              </ol>
            </nav>
            <p class="lead">Found items: {{posts.count}}</p>
            <div v-for="post in posts.results" :key="post.id">
              <nuxt-link :to="`/posts/${post.slug}`"><h2>{{ post.h1 }}</h2></nuxt-link>
              <p v-html="post.description"></p>
              <hr>
            </div>
          </div>
        </div>
      </div>
      <nav aria-label="Paginate me" v-if="total > 1">
      <ul class="pagination justify-content-center">
        <nuxt-link v-if="previous != null" class="page-link" :to="previous" tabindex="-1">Previous</nuxt-link>
        <li v-else class="page-item disabled">
          <a class="page-link disabled" href="#" tabindex="-1">Previous</a>
        </li>
        <span v-for="i in total" :key=i>
          <li  v-if="current_page === i || (!$route.query.page && i === 1)" class="page-item active">
            <nuxt-link class="page-link" :to="`?page=${i}&q=${search_query}`">{{i}}</nuxt-link></li>
          <li v-else class="page-item">
            <nuxt-link class="page-link" :to="`?page=${i}&q=${search_query}`">{{i}}</nuxt-link></li>
        </span>
        <nuxt-link v-if="next != null" class="page-link" :to="next">Next</nuxt-link>
        <li v-else class="page-item disabled">
          <a class="page-link" href="#">Next</a>
        </li>
      </ul>
    </nav>
    <br>
    </div>
  </template>
    
<script>
  import axios from "axios";
  export default {
    layout: "post_detail",
    watch_query: ['q', 'page'],
    watch: {
      $route: function() {
        this.runSearch();
      }
    },
    data() {
      return {
        posts: [],
        total: [],
        next: [],
        previous: [],
        current_page: 0,
        search_query: ''
      }
    },
    methods: {
      async runSearch(){
        let query = this.$route.query;
        let current_page = query.page;
        let search_query = query.q;
        let page_and_search = current_page !== undefined ? `?page=${current_page}&search=${search_query}` : `?search=${search_query}`;
        const { data } = await axios.get(encodeURI(`http://localhost:8000/api/posts/${page_and_search}`));
        let next = data.next != null ? data.next.split('/')[5] : data.next;
        let previous = data.previous != null ? data.previous.split('/')[5] : data.previous;
        
        this.posts= data;
        this.total= Math.ceil(data.count / 6);
        this.next= next;
        this.previous= previous;
        this.current_page= Number(current_page);
        this.search_query= search_query;
      }
    },
    async asyncData({route}) {
      let current_page = route.query.page;
      let search_query = route.query.q;
      console.log(current_page)  
      let page_and_search = current_page !== undefined ? `?page=${current_page}&search=${search_query}` : `?search=${search_query}`;
      console.log(page_and_search)
      const { data } = await axios.get(encodeURI(`http://localhost:8000/api/posts/${page_and_search}`));
      console.log('from server')
      console.log(data)
      let next = data.next != null ? data.next.split('/')[5] : data.next;
      let previous = data.previous != null ? data.previous.split('/')[5] : data.previous;
      return {
        posts: data,
        total: Math.ceil(data.count / 6),
        next: next,
        previous: previous,
        current_page: Number(current_page),
        search_query: search_query
      }
    },
  }
</script>