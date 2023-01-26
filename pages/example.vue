<template>
    <div>
      <ExampleComponent @sendTopage="data_from_component = $event"/>
      <h1>Parent</h1>
      <p>Data from component <b>{{ data_from_component }}</b></p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        data_from_component: ""
      }
    }
  }
  </script>