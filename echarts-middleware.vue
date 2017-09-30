<template>
  <div class="echarts"></div>
</template>

<script>
  import echarts from 'echarts'
  // echarts实例
  export default {
    props: {
      value: null,
      opt: Object,
      h: Number,
      w: Number,
      theme: [String, Object]
    },
    data () {
      return {
        chart: null
      }
    },
    mounted () {
      const opt    = this.opt
      let   width  = this.w,
            height = this.h
      if (!echarts) {
        console.error('本组件需要配合echarts组件使用,请运行npm i -save echarts安装!')
        return null
      }
      // 判断用户是否指定了组件的 宽 和 高，如果指定了，那么使用用户指定的值
      // 如果没有指定则使用 父组件 宽高
      // 如果出错 使用400px默认宽高
      if (!width && !height) {
        const parent = this.$el.parentNode
        if (parent && parent.clientWidth && parent.clientHeight) {
          height = parent.clientHeight
          width = parent.clientWidth
        } else {
          height = width = 400
          console.error('图表 宽度(w) 和 高度(h) 未设置!')
        }
      }
      // 注册echarts
      if (opt) {
        let chart = null
        setTimeout(() => {
          chart = echarts.init(this.$el, this.theme, {width, height})
          // 绘制图表
          chart.setOption(opt)
          this.chart = chart
          // 判断是否绑定v-model，如果绑定了就将值传给v-model
          if (this.value !== undefined) {
            this.$emit('input', chart)
          }
        }, 0)
      }
    },
    beforeDestroy () {
      // 释放echarts
      this.chart.dispose()
    }
  }
</script>
