<template>
<div class='oneword'>
  <!-- <h1>{{word['name']}}</h1> -->
  <div v-for="(letter, index) in letters" :key='index'>
    <div :ref="letter['id']" class="letter letter-outliner">
      {{letter['name']}}
    </div>
  </div>
  <div style="clear:both;"></div>
  <div>
    <input
      class="enter"
      id="enter"
      ref="enter"
      v-model="entered_letter"
      v-on:input="letter_is_enter()"
      v-on:keyup.esc="back()"
    >
  </div>
  <!-- <p>{{letters}}</p> -->
</div>
</template>


<script>
export default {
  name: 'OneWord',
  props:{
    word: {},
    letterSound: {},
  },

  data(){
    return{
        actual_position: 0,
        entered_letter: '',
        theEnd: false,
    }
  },

  mounted: function (){
    this.$refs['r'+String(this.actual_position)][0].classList.value='letter letter-actual';
    this.$refs['enter'].focus();
    setTimeout(()=> {
      this.audioplay(this.word['voice']);
    }, 1000);
    
  },

  computed: {
    letters(){
      let id=0; //start id
      let arr=[];
      for (const item of this.word['name'].toUpperCase().split('')){
        arr.push({'name':item, 'id':'r'+String(id)});
        id++;
      }
      return arr;
    },

    number_letters(){
      return this.word['name'].toUpperCase().length;
    },
  },

  methods:{
    audioplay: function (voice) {
      const audio = new Audio(voice);
      audio.play();
    },

    letter_is_enter: function() {
      if (this.letters[this.actual_position]['name']==this.entered_letter.toUpperCase()){
        this.audioplay(this.letterSound[this.entered_letter.toUpperCase()]);
        this.$refs['r'+String(this.actual_position)][0].classList.value='letter letter-color';
        if (this.actual_position < this.number_letters-1){
          this.$refs['r'+String(this.actual_position+1)][0].classList.value='letter letter-actual';
        }
        if (this.actual_position === this.number_letters-1){
          this.finish();
        }
        this.actual_position++;
      }
        this.entered_letter='';
    },

    finish(){
      setTimeout(()=> {this.theEnd=true;}, 100);
      setTimeout(()=> {
        // alert("МОЛОДЕЦ!!!");
        this.nextWord();
      }, 1000);

    },

    back(){
      setTimeout(()=> {alert("EXIT");}, 10);
    },

    nextWord(){
      this.$emit('nextWord');
    },

  },
}
</script>


<style>
  .letter{
  font-family: "calibri";
  text-align:left;
  vertical-align: bottom;
  font-size:180px;
  font-weight: 700;
  float: left;
  padding: 0px 0px 0px 40px;
  margin: 23px 0px 0px 0px;
}

.letter-outliner{
  color: transparent;
  -webkit-text-stroke: 1px green;
}

.letter-color{
  color: green;
}

.letter-actual{
  margin: 0px 0px 0px 0px;
  font-size:210px;
/*  color: transparent;*/
  color:  #ec7063;
/*  -webkit-text-stroke: 2px red;*/
}

.enter{
  background: transparent;
  border: none;
  outline:none;
}


</style>
