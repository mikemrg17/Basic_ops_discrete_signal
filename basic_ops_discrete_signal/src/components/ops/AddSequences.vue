<template>
    <div>
        Sumar/Restar
        <p>xseq={{xseq}}</p>
        <p>hseq={{hseq}}</p>
        Suma:{{sum}}
        <br>
        Resta:{{sub}}
        <ChartPlot/>
    </div>
</template>

<script>
import ChartPlot from './ChartPlot.vue'

export default {
    name: "AddSequences",
    data(){
        return {
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
    methods:{
    },
    computed:{
        sum(){
            let addition = []
            for(let i = 0; i < this.xseq.length; i++){
                addition.push(this.xseq[i] + this.hseq[i])
            }

            return addition
        },
        sub(){
            let substraction = []
            for(let i = 0; i < this.xseq.length; i++){
                substraction.push(this.xseq[i] - this.hseq[i])
            }
            return substraction
        }
    },
    components:{
        ChartPlot
    }
}
</script>

<style>

</style>