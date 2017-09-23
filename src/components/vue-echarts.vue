<template>
  <div class="echarts"></div>
</template>

<script>
  import echarts from 'echarts/lib/echarts'
  export default {
    props: {
      options: Object
    },
    data () {
      return {
        chart: null
      }
    },
    mounted () {
      if (this.options) {
        let chart = echarts.init(this.$el, this.options)
      }
    }
  }
</script>