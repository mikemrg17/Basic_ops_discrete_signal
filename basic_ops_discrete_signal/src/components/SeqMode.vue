<template>
  <div>
    Valor de x(n): <input type="number" class="seq-len" v-model="xValue"> <button class="add" @click="AddXValue">Agregar</button>
    <br>
    Índice de x(n) con n=0 : <input type="number" class="seq-len" v-model="x0Value" min="0">
    <br>
    <br>
    Valor de h(n): <input type="number" class="seq-len" v-model="hValue"> <button class="add" @click="AddHValue">Agregar</button>
    <br>
    Índice de h(n) con n=0 : <input type="number" class="seq-len" v-model="h0Value" min="0">
    <br>
    <br>
    <br>
    <b>Secuencias de las señales:</b>
    <div class="sequence">
        x(n) = 
        <div v-for="index in XSeq.length" :key="index">
            <NumSlot :value="XSeq[index-1]"/>
        </div>
        <br>
    </div>
    x(n=0) : {{x0Value}}
    <br>
    <div class="sequence">
        h(n) = 
        <div v-for="idx in HSeq.length" :key="idx">
            <NumSlot :value="HSeq[idx-1]"/>
        </div>
        <br>
    </div>
    h(n=0): {{h0Value}}
    <br>
    <div id="charts" v-show="showCharts == 1">
        X(n):
        <ChartPlot :height="2" :width="4" :datasetIdKey="'X(n)'" :sequences="XSeqArray"/>
        H(n):
        <ChartPlot :height="2" :width="4" :datasetIdKey="'H(n)'" :sequences="HSeqArray"/>
    </div>
    <br>
    <br>
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
    <div v-if="showOps == 1">
        <div v-show="OpOpt == 1" class="ops">
            <AddSequences :xseq="XSeq" :hseq="HSeq" :x0value="Number(x0Value)" :h0value="Number(h0Value)"/>
        </div>
        <div v-show="OpOpt == 2" class="ops">
            <AmpSequences :xseq="XSeq" :hseq="HSeq" :x0value="Number(x0Value)" :h0value="Number(h0Value)"/>
        </div>
        <div v-show="OpOpt == 3" class="ops">
            <ReflexSequences :xseq="XSeq" :hseq="HSeq" :x0value="Number(x0Value)" :h0value="Number(h0Value)"/>
        </div>
        <div v-show="OpOpt == 4" class="ops">
            <DisplaceSequences :xseq="XSeq" :hseq="HSeq" :x0value="Number(x0Value)" :h0value="Number(h0Value)"/>
        </div>
        <div v-show="OpOpt == 5" class="ops"></div>
        <div v-show="OpOpt == 6" class="ops"></div>
    </div>
  </div>
</template>

<script>
import NumSlot from './NumSlot.vue'
import AddSequences from './ops/AddSequences.vue'
import ChartPlot from './ops/ChartPlot.vue'
import AmpSequences from './ops/AmpSequences.vue'
import ReflexSequences from './ops/ReflexSequences.vue'
import DisplaceSequences from './ops/DisplaceSequences.vue'

export default {
    name: 'SeqMode',
    components:{
    NumSlot,
    ChartPlot,
    AddSequences,
    AmpSequences,
    ReflexSequences,
    DisplaceSequences
},
    data(){
        return {
            xValue: 0,
            hValue: 0,
            x0Value: 0,
            h0Value: 0,
            XSeq: [],
            HSeq: [],
            OpOpt: 0,
            showOps: 0,
            showCharts : 0,
            XSeqArray: [],
            HSeqArray: []
        }
    },
    methods:{
        AddXValue(){
            this.XSeq.push(Number(this.xValue))
            this.xValue = 0
        },
        AddHValue(){
            this.HSeq.push(Number(this.hValue))
            this.hValue = 0
        },
        AcceptValues(){
            if(this.x0Value > this.XSeq.length-1 || this.h0Value > this.HSeq.length-1){
                alert("Asegúrate de no poner un n=0 mayor a la cantidad de índices de la secuencia")
            }else{
                this.balanceSeq()
                this.fillSeqArrays()
                this.showOps = 1
            }
        },
        CleanFields(){
            this.XSeq = []
            this.HSeq = []
            this.OpOpt = 0
            this.x0Value = 0
            this.h0Value = 0
            this.showOps = 0
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
        balanceSeq(){
            let maxN0 = this.max0(this.x0Value, this.h0Value)
            if(maxN0 == 'x'){
                let auxN0 = this.h0Value
                while(auxN0 < this.x0Value){
                    this.HSeq.unshift(0)
                    auxN0++
                }
                this.h0Value = auxN0
            }else if(maxN0 == 'h'){
                let auxN0 = this.x0Value
                while(auxN0 < this.h0value){
                    this.XSeq.unshift(0)
                    auxN0++
                }
                this.x0Value = auxN0
            }
            
            let maxSeqLength = this.maxL(this.XSeq, this.HSeq)
            if(maxSeqLength == 'x'){
                let auxLen = this.HSeq.length
                while(auxLen < this.XSeq.length){
                    this.HSeq.push(0)
                    auxLen++
                }
            }else if(maxSeqLength == 'h'){
                let auxLen = this.XSeq.length
                while(auxLen < this.HSeq.length){
                    this.XSeq.push(0)
                    auxLen++
                }
            }
        },
        maxL: function(xSequence, hSequence){
            let maxS
            if(xSequence.length > hSequence.length)
                maxS = 'x'
            else if(xSequence.length == hSequence.length)
                maxS = 'b'
            else
                maxS = 'h'
            
            return maxS
        },
        max0(x0, h0){
            let maxN = ''
            if(x0 > h0)
                maxN = 'x'
            else if(x0 == h0)
                maxN = 'b'
            else
                maxN = 'h'
            
            return maxN
        },
        DisplayCharts(){
            this.showCharts = !this.showCharts
        },
        fillSeqArrays(){
            let xAxises = 0 - this.x0Value
            for(let i = 0; i < this.XSeq.length; i++){
                this.XSeqArray.push({
                    x: xAxises,
                    y: this.XSeq[i]
                })
                xAxises++
            }

            xAxises = 0 - this.h0Value
            for(let i = 0; i < this.HSeq.length; i++){
                this.HSeqArray.push({
                    x: xAxises,
                    y: this.HSeq[i]
                })
                xAxises++
            }
        }
        
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

    .add0{
        border: none;
        border-radius: 0.25em;
        background-color: #008080;
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

    .seq-len{
        width: 3em;
    }

    #charts {
        margin: auto;
        width: 60%;
    }
</style>