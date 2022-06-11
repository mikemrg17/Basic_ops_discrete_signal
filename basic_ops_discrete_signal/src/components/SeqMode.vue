<template>
  <div>
    Valor de x(n): <input type="number" class="seq-len" v-model="xValue"> <button class="add" @click="AddXValue">Agregar</button>
      <br>
      <br>
    Valor de h(n): <input type="number" class="seq-len" v-model="hValue"> <button class="add" @click="AddHValue">Agregar</button>
    <br>
    <br>
    Secuencias de las señales:
    <div class="sequence">
        x(n) = 
        <div v-for="index in XSeq.length" :key="index">
            <NumSlot :value="XSeq[index-1]"/>
        </div>
    </div>
    <div class="sequence">
        h(n) = 
        <div v-for="idx in HSeq.length" :key="idx">
            <NumSlot :value="HSeq[idx-1]"/>
        </div>
    </div>
    <button id="btn-accept" @click="AcceptValues">Aceptar</button>
    <button id="btn-clean" @click="CleanFields">Limpiar</button>
    <button id="btn-graph" @click="DisplayCharts">Graficar</button>
    <div id="opt-nav">
      <div class="opt" @click="changeAdd">Suma/Resta</div>
      <div class="opt" @click="changeAmp">Amplificación/Atenuación</div>
      <div class="opt" @click="changeRef">Reflejo</div>
      <div class="opt" @click="changeDis">Desplazamiento</div>
      <div class="opt" @click="changeDec">Diezmación/Interpolación</div>
      <div class="opt" @click="changeCon">Convolución</div>
    </div>
    <hr>
    <div v-show="OpOpt == 1" class="ops">
        <AddSequences :xseq="XSeq" :hseq="HSeq"/>
    </div>
    <div v-show="OpOpt == 2" class="ops"></div>
    <div v-show="OpOpt == 3" class="ops"></div>
    <div v-show="OpOpt == 4" class="ops"></div>
    <div v-show="OpOpt == 5" class="ops"></div>
    <div v-show="OpOpt == 6" class="ops"></div>
  </div>
</template>

<script>
import NumSlot from './NumSlot.vue'
import AddSequences from './ops/AddSequences.vue'

export default {
    name: 'SeqMode',
    components:{
    NumSlot,
    AddSequences,
},
    data(){
        return {
            xValue: 0,
            hValue: 0,
            XSeq: [],
            HSeq: [],
            OpOpt: 0
        }
    },
    methods:{
        AddXValue(){
            this.XSeq.push(this.xValue)
            this.xValue = 0
        },
        AddHValue(){
            this.HSeq.push(this.hValue)
            this.hValue = 0
        },
        AcceptValues(){},
        CleanFields(){
            this.XSeq = []
            this.HSeq = []
            this.OpOpt = 0
        },
        changeAdd(){
            this.OpOpt = 1
        },
        changeAmp(){
            this.OpOpt = 2
        },
        changeRef(){
            this.OpOpt = 3
        },
        changeDis(){
            this.OpOpt = 4
        },
        changeDec(){
            this.OpOpt = 5
        },
        changeCon(){
            this.OpOpt = 6
        },
    }
}
</script>

<style scoped>
    .sequence{
        display: flex;
    }

    .add{
        border: none;
        border-radius: 0.25em;
        background-color: #566573;
        color: white;
    }

    #btn-accept{
        border: none;
        border-radius: 0.25em;
        background-color: #17A589;
        color: white;
    }

    #btn-clean{
        margin-left: 1em;
        border: none;
        border-radius: 0.25em;
        background-color: #A93226;
        color: white;
    }

    #btn-graph{
        margin-left: 1em;
        border: none;
        border-radius: 0.25em;
        background-color: #884EA0;
        color: white;
    }

    #slots{
        display: flex;
        flex-direction: row;
    }

    #opt-nav {
        margin-top: 1em;
        width: 100%;
        display: flex;
        flex-direction: row;
    }

    .opt {
        text-align: center;
        padding: 10px;
        margin: auto;
        width: 50%;
        height: 3em;
    }

    .opt:hover {
        cursor:pointer;
        border-radius: 0.5em;
        background-color: #D5D8DC;
    }
</style>