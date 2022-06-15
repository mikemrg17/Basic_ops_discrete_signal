<template>
    <div>
        Suma: {{Addition}}
        <ChartPlot v-if="SumSeqArray != []" :height="2" :width="4" :datasetIdKey="'Suma'" :sequences="SumSeqArray"/>
        <br>
        Resta: {{Substraction}}
        <ChartPlot v-if="SubSeqArray != []" :height="2" :width="4" :datasetIdKey="'Resta'" :sequences="SubSeqArray"/>
    </div>
</template>

<script>
import ChartPlot from './ChartPlot.vue'

export default {
    name: "AddSequences",
    data(){
        return {
            Addition: [],
            Substraction: [],
            SumSeqArray: [],
            SubSeqArray: []
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
    mounted(){
        let addition = []
        for(let i = 0; i < this.xseq.length; i++){
            addition.push(this.xseq[i] + this.hseq[i])
        }
        this.Addition = addition

        let substraction = []
        for(let i = 0; i < this.xseq.length; i++){
            substraction.push(Number(this.xseq[i] - this.hseq[i]))
        }
        this.Substraction = substraction

        let xAxises = 0 - this.x0value
        for(let i = 0; i < this.Addition.length; i++){
            this.SumSeqArray.push({
                x: Number(xAxises),
                y: Number(this.Addition[i])
            })
            xAxises++
        }

        xAxises = 0 - this.x0Value
        for(let j = 0; j < this.Substraction.length; j++){
            this.SubSeqArray.push({
                x: Number(xAxises),
                y: Number(this.Substraction[j])
            })
            xAxises++
        }
    }
}
</script>

<style>

</style>