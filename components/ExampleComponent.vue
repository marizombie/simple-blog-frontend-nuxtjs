<template>
  <div>
    <h1>Child component</h1>
    <input type="text" placeholder="Text for sending to page:" v-model="text">
    <button @click='sendTopage()'>Send to page</button>
  </div>
</template>

<script>
export default {
  data(){
    return {
      text: ''
    }
  },
  methods: {
    sendTopage() {
      this.$emit('sendTopage', this.text)
    }
  }
}
</script>