<template>
    <div>
        <button class="add" @click="ReflexSeqs">Reflejar</button>
        <div v-show="showCharts == 1">
            <ChartPlot :height="2" :width="4" :datasetIdKey="'X(n)'" :sequences="RxSeq"/>
            <br>
            <ChartPlot :height="2" :width="4" :datasetIdKey="'H(n)'" :sequences="RhSeq"/>
        </div>
    </div>
</template>

<script>
import ChartPlot from './ChartPlot.vue'

export default {
    name: "ReflexSequences",
    data(){
        return {
            showCharts: 0,
            RmX: [],
            RmH: [],
            RxSeq: [],
            RhSeq: [],
            NewX0: 0,
            NewH0: 0
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
            let rX = []
            for(let i = 0; i < this.xseq.length; i++){
                rX.unshift(this.xseq[i])
            }
            this.RmX = rX

            let newX0 = this.xseq.length - this.x0value
            this.NewX0 = newX0

            let rH = []
            for(let i = 0; i < this.hseq.length; i++){
                rH.unshift(this.hseq[i])
            }
            this.RmH = rH

            let newH0 = this.hseq.length - this.h0value
            this.NewH0 = newH0
        },
        makeObjSeq(){
            let xAxises = 0 - this.NewX0
            for(let i = 0; i < this.RmX.length; i++){
                this.RxSeq.push({
                    x: Number(xAxises),
                    y: Number(this.RmX[i])
                })
                xAxises++
            }

            xAxises = 0 - this.NewH0
            for(let j = 0; j < this.RmH.length; j++){
                this.RhSeq.push({
                    x: Number(xAxises),
                    y: Number(this.RmH[j])
                })
                xAxises++
            }
        },
        ReflexSeqs(){
            this.updateA()
            this.makeObjSeq()
            this.showCharts = 1
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