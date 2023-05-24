<template>
<div class='oneword'>
  <h1>{{word}}</h1>
  <div v-for="(letter, index) in letters" :key='index'>
    <div :ref="letter['id']" class="letter letter-outliner">
      {{letter['name']}}
    </div>
  </div>
  <div style="clear:both;"></div>
  <div>
    <input  v-show=true v-focus id="enter" v-model="entered_letter" v-on:input="letter_is_enter()">
  </div>
  <p>{{letters}}</p>
</div>
</template>


<script>
export default {
  name: 'OneWord',
  
  data(){
    return{
        word: 'колбаса',
        actual_position: 0,
        entered_letter: '',
        theEnd: false,
    }
  },

  computed: {
    letters(){
      let id=0; //start id
      let arr=[];
      for (const item of this.word.toUpperCase().split('')){
        arr.push({'name':item, 'id':'r'+String(id)});
        id++;
      }
      return arr;
    },

    number_letters(){
      return this.word.toUpperCase().length;
    },
  },

  methods:{
    audioplay: function (voice) {
      const audio = new Audio(voice);
      audio.play();
    },

    letter_is_enter: function() {
      if (this.letters[this.actual_position]['name']==this.entered_letter.toUpperCase()){
        // this.audioplay(this.voices[this.entered_letter.toUpperCase()]);
        this.$refs['r'+String(this.actual_position)][0].classList.value='letter letter-color';
        if (this.actual_position < this.number_letters-1){
          this.$refs['r'+String(this.actual_position+1)][0].classList.value='letter letter-actual';
        }
        if (this.actual_position === this.number_letters-1){
          // setTimeout(() => this.finish(), 2000);
          this.finish();
        }
        this.actual_position++;
      }
        this.entered_letter='';
    },

    finish(){
      setTimeout(()=> {this.theEnd=true;}, 2000);
      setTimeout(()=> {alert("МОЛОДЕЦ!!!");}, 2000);
    },
  },
}
</script>


<style>
  .letter{
  text-align:left;
  vertical-align: bottom;
  font-size:150px;
  font-weight: 700;
  float: left;
  padding: 0px 0px 0px 0px;
  margin: 23px 20px 0px 0px;
}

.letter-outliner{
  color: transparent;
  -webkit-text-stroke: 0.5px green;
}

.letter-color{
  color: green;
}

.letter-actual{
  margin: 0px 0px 0px 0px;
  font-size:170px;
  color: transparent;
  -webkit-text-stroke: 0.5px green;
}
</style>
