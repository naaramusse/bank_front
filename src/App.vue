<template>
  <img alt="Vue logo" src="./assets/logo.png" style="width: 100px">
  <Bank msg="Bem vindo ao Banco Capgemini"/>
</template>

<script>
  import 'bootstrap/dist/css/bootstrap.css'
  import 'bootstrap-vue/dist/bootstrap-vue.css'

  import Bank from './components/Bank.vue'

  export default {
    name: 'App',
    components: {
      Bank
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
