<template>
    <div>
        <input type="number" v-model="Displace" class="seq-len"/> <button class="add" @click="DisSeqs">Desplazar</button>
        <div v-show="showCharts == 1">
            X(n): {{DmX}}
            <ChartPlot :height="2" :width="4" :datasetIdKey="'X(n)'" :sequences="DxSeq"/>
            <br>
            H(n): {{DmH}}
            <ChartPlot :height="2" :width="4" :datasetIdKey="'H(n)'" :sequences="DhSeq"/>
        </div>
    </div>
</template>

<script>
import ChartPlot from './ChartPlot.vue'

export default {
    name: "DisplaceSequences",
    data(){
        return {
            showCharts: 0,
            Displace: 0,
            DmX: [],
            DmH: [],
            DxSeq: [],
            DhSeq: [],
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
        updateD(){
            this.NewX0 = this.x0value - this.Displace;
            this.NewH0 = this.h0value - this.Displace;

            let dX = this.xseq

            if(this.NewX0 < 0){
                for(let i = 0; i < Math.abs(this.NewX0); i++){
                    dX.unshift(0)
                }
                this.NewX0 = 0
            }else if(this.NewX0 > this.xseq.length){
                for(let i = 0; i < Math.abs(this.NewX0-this.xseq.length); i++){
                    dX.push(0)
                }
                this.NewX0 = this.dX.length
            }

            this.DmX = dX

            let dH = this.hseq

            if(this.NewH0 < 0){
                for(let i = 0; i < Math.abs(this.NewH0); i++){
                    dH.unshift(0)
                }
                this.NewH0 = 0
            }else if(this.NewH0 > this.hseq.length){
                for(let i = 0; i < Math.abs(this.NewH0-this.hseq.length); i++){
                    dH.push(0)
                }
                this.NewH0 = this.dH.length
            }

            this.Dmh = dH

        },
        makeObjSeq(){
            let xAxises = 0 - this.NewX0
            for(let i = 0; i < this.DmX.length; i++){
                this.DxSeq.push({
                    x: Number(xAxises),
                    y: Number(this.DmX[i])
                })
                xAxises++
            }

            xAxises = 0 - this.NewH0
            for(let j = 0; j < this.DmH.length; j++){
                this.DhSeq.push({
                    x: Number(xAxises),
                    y: Number(this.DmH[j])
                })
                xAxises++
            }
        },
        DisSeqs(){
            this.updateD()
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
    .seq-len{
        width: 3em;
    }
</style>