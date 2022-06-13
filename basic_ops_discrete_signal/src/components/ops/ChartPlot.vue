<template>
  <Plotly :data="data" :layout="layout" />
</template>

<script>
import { Plotly } from 'vue-plotly'

export default {
    name: 'ChartPlot',
    components: {
        Plotly
    },
    data(){
        return {
            data: [{
                x:[1,2,3,4],
                y:[10,15,13,17],
                type:"histogram"
            }],
            layout:{
                title: "test graph"
            }
        }
    }

}
</script>

<style>

</style>