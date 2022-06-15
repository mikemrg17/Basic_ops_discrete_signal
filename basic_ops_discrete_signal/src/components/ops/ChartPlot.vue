<template>
  <Scatter
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="width"
    :height="height"
  />
</template>

<script>
import { Scatter } from 'vue-chartjs/legacy'

import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, CategoryScale, PointElement, LinearScale, Filler } from 'chart.js'

ChartJS.register( Title, Tooltip, Legend, LineElement, CategoryScale, PointElement, LinearScale, Filler)

export default {
  name: 'ChartPlot',
  components: {
    Scatter
  },
  props: {
    chartId: {
      type: String,
      default: 'scatter-chart'
    },
    datasetIdKey: {
      type: String,
      default: 'label'
    },
    width: {
      type: Number,
      default: 4
    },
    height: {
      type: Number,
      default: 5
    },
    cssClasses: {
      default: '',
      type: String
    },
    styles: {
      type: Object,
      default: () => {}
    },
    plugins: {
      type: Array,
      default: () => []
    },
    sequences:{
        type: Array,
        default: () => []
    }
  },
  data() {
    return {
      chartData: {
        datasets: [
          {
            label: this.datasetIdKey,
            borderColor: '#f87979',
            backgroundColor: '#f87979',
            data: []
          }
        ]
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      }
    }
  },
  mounted(){
    this.chartData.datasets[0].data = this.sequences
  }
}
</script>
