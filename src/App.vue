<template>
<div id='app'>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <OneWord
    :word="word"
  >
  </OneWord>
</div>
</template>

<script>
import OneWord from './components/word.vue'
export default {
  name: 'App',
  components: {
    OneWord,
  },

  data(){
    return{
      word:'колбаса',
    }
  },
}
</script>

<style>
#app {
background: #ddecf9;
margin: 0;
}
</style>
