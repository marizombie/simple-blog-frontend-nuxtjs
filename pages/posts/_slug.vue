<template>
    <div>
        <Header :h1=post.h1 />
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <nav aria-label="breadcrumb" class="mt-4">
                        <ol class="breadcrumb" itemscope itemtype="https://schema.org/BreadcrumbList">
                            <li class="breadcrumb-item" itemprop="itemListElement" 
                            itemscope itemtype="https://schema.org/ListItem">
                                <nuxt-link to="/" itemid="/" itemtype="https://schema.org/Thing" itemscope itemprop="item">
                                    <span itemprop="name">Home</span>
                                </nuxt-link>
                                <meta itemprop="position" content="1">
                            </li>
                            <li class="breadcrumb-item active" aria-current="page" itemprop="itemListElement" 
                            itemscope itemtype="https://schema.org/ListItem">
                                <span itemprop="name">{{ post.h1 }}</span>
                                <meta itemprop="position" content="2">
                            </li>
                        </ol>
                    </nav>
                    <img class="img-fluid rounded " :src="post.image" alt="">
                    <hr>
                    <p v-html="post.content"></p>
                    <div class="d-flex justify-content-end">
                        <span v-for="tag in post.tags" :key="tag.name">
                            <nuxt-link :to="`/tags/${tag}`" class="mr-1 badge badge-info">#{{ tag }}</nuxt-link>
                        </span>
                    </div>
                    <hr>
                    <div class="d-flex">
                        <div class="mr-auto p-2 lead">Author: {{ post.author }}</div>
                        <div class="p-2">Published: {{ post.created_at }}</div>
                    </div>
                    <hr>
                    <Comments :comments=comments :post=post />
                </div>
                <Aside :tags=tags :aside=aside />
            </div>
        </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";

  export default {
    layout: "post_detail",
    async asyncData({params}) {
        console.log(params);
        const post = await axios.get(`http://127.0.0.1:8000/api/posts/${params.slug}`);
        const tags = await axios.get(`http://127.0.0.1:8000/api/tags`);
        const aside = await axios.get(`http://127.0.0.1:8000/api/last-posts/`);
        const comments = await axios.get(`http://127.0.0.1:8000/api/comments/${params.slug}`);
        return {
            post: post.data,
            tags: tags.data,
            aside: aside.data,
            comments: comments.data,
        }
    },
    head() {
        return {
            title: this.post.title,
            meta: [
            {
                hid: "description",
                name: "description",
                content: this.post.description
            },
            {
                property: "og:url",
                content: `http://localhost:3000${this.$route.path}`
                },
                {
                property: "og:type",
                content: "website"
                },
                {
                property: "og:title",
                content: `${this.post.title}`
                },
                {
                property: "og:description",
                content: `${this.post.description}`
                },
                {
                property: "og:site_name", content: "Interview preparation wow"
                },
                {
                property: "og:locale", content: "en_US"
                },
                {
                property: "og:image",
                content: "???????????? ???? ????????????????"
                },
                {
                property: "og:image:alt",
                content: "Best materials, tips and tutorials for you"
                },
                {
                property: "fb:app_id", content: "23456789"
                },
                {
                name: "twitter:card", content: "summary_large_image"
                },
                {
                name: "twitter:site", content: "http://localhost:3000"
                },
                {
                name: "twitter.title",
                content: `${this.post.title}`
                },
                {
                name: "twitter:description",
                content: `${this.post.description}`
                },
                {
                name: "twitter:image:src",
                content: "http://localhost:3000/img"
                }
            ]
        };
    },
  }
  </script>