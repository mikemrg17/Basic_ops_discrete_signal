<template>
    <div>
        <input type="number" v-model="valueA"><button class="add" @click="ASeqs">Amplificar/Atenuar</button>
        <div v-show="showCharts == 1">
            X(n): {{AmX}}
            <ChartPlot :height="2" :width="4" :datasetIdKey="'X(n)'" :sequences="AxSeq"/>
            <br>
            H(n): {{AmH}}
            <ChartPlot :height="2" :width="4" :datasetIdKey="'H(n)'" :sequences="AhSeq"/>
        </div>
    </div>
</template>

<script>
import ChartPlot from './ChartPlot.vue'

export default {
    name: "AmpSequences",
    data(){
        return {
            valueA: 0,
            showCharts: 0,
            AmX: [],
            AmH: [],
            AxSeq: [],
            AhSeq: [],
        }
    },
    props:{
        xseq: {
            type: Array,
            default: () => []
        },
        hseq: {
            type: Array,
            default: () => []
        },
        x0value: {
            type: Number,
            default: 0
        },
        h0value: {
            type: Number,
            default: 0
        }
    },
    components:{
        ChartPlot
    },
    methods:{
        updateA(){
            let aX = []
            for(let i = 0; i < this.xseq.length; i++){
                aX.push(this.xseq[i] * this.valueA)
            }
            this.AmX = aX

            let aH = []
            for(let i = 0; i < this.hseq.length; i++){
                aH.push(Number(this.hseq[i] * this.valueA))
            }
            this.AmH = aH
        },
        makeObjSeq(){
            let xAxises = 0 - this.x0value
            for(let i = 0; i < this.AmX.length; i++){
                this.AxSeq.push({
                    x: Number(xAxises),
                    y: Number(this.AmX[i])
                })
                xAxises++
            }

            xAxises = 0 - this.h0Value
            for(let j = 0; j < this.AmH.length; j++){
                this.AhSeq.push({
                    x: Number(xAxises),
                    y: Number(this.AmH[j])
                })
                xAxises++
            }
        },
        ASeqs(){
            this.updateA()
            this.makeObjSeq()
            this.showCharts = 1;
        }
    }
}
</script>

<style scoped>
    .add{
        margin-left: 1em;
        border: none;
        border-radius: 0.25em;
        background-color: #566573;
        color: white;
    }
</style>