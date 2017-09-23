<template>
  <div :style="{ height: height + 'px', width: width +'px' }"></div>
</template>

<script>
  import echarts from 'echarts'
  // echarts实例
  let chart = null

  export default {
    props: {
      opt: Object,
      h: Number,
      w: Number
    },
    data () {
      return {
        height: 0,
        width: 0
      }
    },
    created () {
      if (!echarts) {
        console.error('本组件需要配合echarts组件使用,请运行npm i -save echarts安装!')
      }
      const h = this.h
      const w = this.w
      // 判断用户是否指定了组件的 宽 和 高，如果指定了，那么使用用户指定的值
      // 如果没有指定则使用 父组件 宽高
      // 如果出错 使用400px默认宽高 
      if (h && w) {
        this.height = h
        this.width = w
      } else {
        const parent = this.$parent
        if (parent && parent.width && parent.height) {
          this.height = parent.height
          this.width = parent.width
        } else {
          this.height = this.width = 400
          console.error('图表 宽度(w) 和 高度(h) 未设置!')
        }
      }
    },
    mounted () {
      // 注册echarts
      if (this.opt) {
        chart = echarts.init(this.$el, this.opt)
        // 绘制图表
        chart.setOption(this.opt)
      }
    },
    computed: {
      // 返回echarts实例
      chart () {
        return chart
      }
    },
    beforeDestroy () {
      // 释放echarts
      chart.dispose()
    }
  }
</script>
