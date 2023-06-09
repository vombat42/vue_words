<template>
<div id='app'>
<div id='app2'>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <OneWord
    :key='OneWordKey'
    :word="words[OneWordKey]"
    :letterSound="letters[OneWordKey]"
    @nextWord="nextWord"
  >
  </OneWord>
</div>
</div>
</template>

<script>
import axios from 'axios';
import OneWord from './components/word.vue'
export default {
  name: 'App',
  components: {
    OneWord,
  },

  data(){
    return{
      words:[],
      OneWordKey:0,
      letters:[],
    }
  },

  created: function () {
    const vm=this;
    let bukvi={};
    axios.get(`http://192.168.1.3:8000/games/api/words/`)
    .then(function(response){
      for (const item of response.data) {
        vm.words.push({'name':item['name'], 'voice':item['voice']});
        bukvi={};
        for (const subitem of item['letters']) {
          bukvi[subitem['name']]=subitem['voice'];
        }
        vm.letters.push(bukvi);
      }
    });
  },

  computed:{
    numberWords(){
      return this.words.length;
    },
  },

  methods: {
    nextWord(){
      if (this.OneWordKey < this.numberWords-1) {
        console.log(this.numberWords);
        this.OneWordKey += 1; //следующее слово и перерендерим компонент 
      }
      else {
        alert("Конец!");
      }
    },
  },

}
</script>

<style>

body {
  background: black;
}

#app {
background: #red;
padding: 0px 0px 0px 0px;
margin: 200px 0px 0px 0px;
}

#app2 {
background: #fae5d3;
padding: 0px 0px 0px 0px;
width: 1100px;
/*margin: 0px 0px 0px 0px;*/
margin:0 auto;
}
</style>
