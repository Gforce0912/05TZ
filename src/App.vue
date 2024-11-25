<template>
  <div id="app">
    <Editor />
  </div>
</template>

<script>
import Editor from './components/Editor.vue'

export default {
  name: 'App',
  components: {
    Editor
  }
}
</script>

<style>
  html{
    background: #1E1E1E;
  }
  *{
    color: #EAEAEA;
    font-family: Roboto;
    text-decoration: none;
  }
  p{
    font-size: 15px;
  }
  h1{
    font-size: 31px;
  }
  #app{
    display: flex;
    justify-content: center;
  }
</style>