<template>
  <div class="echarts" :style="{ height: h + 'px', width: w +'px' }"></div>
</template>

<script>
  import echarts from 'echarts'
  let myChart = null
  export default {
    props: {
      opt: Object,
      h: {
        type: Number,
        default: 400,
        validator: (value) => {
          return value >= 0
        }
      },
      w: {
        type: Number,
        default: 400,
        validator: (value) => {
          return value >= 0
        }
      }
    },
    data () {
      return {
        chart: null
      }
    },
    mounted () {
      // 注册echarts
      if (this.opt) {
        let chart = null
        chart = echarts.init(this.$el, this.opt)
        // 绘制图表
        chart.setOption(this.opt)
        this.chart = chart
        console.log(chart)
      }
    },
    beforeDestroy () {
      // 释放echarts
      this.chart.dispose()
    }
  }
</script>

<style>
.echarts {
  position: relative;
}
</style>
