<template>
  <div class="hello">
    <h1>Bienvenidos</h1>
    <h3>Este programa realiza operaciones básicas con señales discretas</h3>
    <br>
    <p>Para comenzar a usar el programa, introduce una secuencia o un audio</p>
    <hr>
    <div id="opt-nav">
      <div class="opt" @click="changeSeq">Secuencia</div>
      <div class="opt" @click="changeAud">Audio</div>
    </div>
    <div v-show="optSelected == 0" class="signals">
      <SeqMode/>
    </div>
    <div v-show="optSelected == 1" class="signals">
      <AudMode/>
    </div>
    
  </div>
</template>

<script>
import SeqMode from './SeqMode.vue'
import AudMode from './AudMode.vue'

export default {
  name: 'MainPage',
  data() {
    return{
      optSelected: 0,
    }
  },
  components: {
    SeqMode,
    AudMode
},
  props: {
  },
  methods:{
    changeSeq(){
      this.optSelected = 0
    },
    changeAud(){
      this.optSelected = 1
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }

  .signals {
    margin-top: 3em;
    margin-left: 1em;
    text-align: left;
  }

  .seq-len {
    width: 3em;
  }

  #opt-nav {
    width: 100%;
    display: flex;
    flex-direction: row;
  }

  .opt {
    padding: 10px;
    margin: auto;
    width: 50%;
    height: 3em;
  }

  .opt:hover {
    border-bottom: 2px solid #566573;
  }

</style>
