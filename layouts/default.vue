<template>
  <div>
    <Navbar />
    <!-- <Slider /> -->
    <div class="container">
      <Nuxt />
    </div>
    <Footer />
  </div>
</template>

<script>
export default {
  head() {
    return {
      link: [
        { rel: "canonical", href: `http://localhost:3000${this.$route.path}`}
      ]
    }
  },
}
</script>
