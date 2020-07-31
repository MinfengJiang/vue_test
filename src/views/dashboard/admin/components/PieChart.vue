<template>
  <div :class="className" :style="{height:height,width:width}" />
</template>

<script>
import echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import resize from './mixins/resize'

export default {
  mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '250px'
    }
  },
  data() {
    return {
      chart: null,
      option: null
    }
  },
  watch: {
    '$i18n.locale'() {
      this.chart.setOption(this.option)
    }
  },
  mounted() {
    this.option = {
      tooltip: {
        trigger: 'item',
        formatter: (params) => {
          return `${this.$t(params.seriesName)} <br/> ${this.$t(params.name)} : ${this.$t(params.value)} (${params.percent}%)`
        }
      },
      legend: {
        left: 'center',
        bottom: '15',
        data: ['dashboard.chartLegend', 'dashboard.chartLegend2'],
        formatter: (params) => {
          return this.$t(params)
        },
        textStyle: {
          fontWeight: 600,
          color: '#444'
        }
      },
      series: [
        {
          name: 'dashboard.chartTitle',
          type: 'pie',
          // roseType: 'radius',
          radius: [45, 95],
          center: ['50%', '43%'],
          data: [
            { value: 320, name: 'dashboard.chartLegend' },
            { value: 24, name: 'dashboard.chartLegend2' }
          ],
          label: {
            normal: {
              formatter: (params) => {
                return this.$t(params.name)
              },
              position: 'inside',
              color: '#666',
              fontWeight: 600
            }
          },
          animationEasing: 'cubicInOut',
          animationDuration: 500
        }
      ]
    }
    this.$nextTick(() => {
      this.initChart()
    })
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    initChart() {
      this.chart = echarts.init(this.$el, 'macarons')
      this.chart.setOption(this.option)
    }
  }
}
</script>
