# echarts-middleware
在vue中优雅，高效的使用echarts

## 快速上手

安装

```
npm install -save echarts
npm install -save echarts-middleware
```

引入并注册

```
<script>
  import echarts from 'echarts'
  import Chart from 'echarts-middleware'
  export default {
    components: {
      Chart
    }
  }
</script>
```

使用
```
<template>
  <div class="my-chart">
    <Chart :opt="data", :w="400", :h="400"></Chart>
  <div>
</template>

<script>
  import echarts from 'echarts'
  import Chart from 'echarts-middleware'
  export default {
    components: {
      Chart
    },
    data () {
      return {
        data: {
          'series': [{
            'name': 'gauge',
            'type': 'gauge',
            'detail': {'formatter': '{value}'},
            'data': [{'value': 34}]
          }]
        }
      }
    }
  }
</script>
```
结果
![结果](http://my-owo-ink.b0.upaiyun.com/puge/github/echarts-middleware/1.png)

## 组件介绍

特点:
* 高效: 直接将echarts实例暴露给vue,可以直接操作
* 优雅: 自动处理echarts的 注册 及 销毁 操作
* 简洁: 主要代码少于50行 没有性能损失
* 接口丰富: 由于直接暴露 echarts实例 所以理论上能用 100% echarts方法

代码对比:

传统vue中使用echarts
```
<template>
  <div class="box">
    <div class="charts" v-for="item in 6" :id="'chart' + item" :key="item"></div>
  </div>
</template>

<script>
  import echarts from 'echarts'
  let myChart = []
  export default {
    data () {
      return {
        mock: {
          'series': [{
            'name': 'gauge',
            'type': 'gauge',
            'detail': {'formatter': '{value}'},
            'data': [{'value': 34}]
          }]
        }
      }
    },
    created () {
      for (let i = 0; i < 6; i++) {
        myChart[i] = echarts.init(document.getElementById('chart' + i))
        myChart[i].setOption(this.mock)
      }
    },
    beforeDestroy () {
      for (let i = 0; i < 6; i++) {
        myChart[i].dispose()
      }
    }
  }
</script>
```

使用echarts-middleware达到同样的效果

```
<template>
  <div class="box">
    <Chart class="charts" v-for="item in 6" :opt="mock" :key="item"></div>
  </div>
</template>

<script>
  import echarts from 'echarts'
  import Chart from 'echarts-middleware'
  export default {
    components: {
      Chart
    },
    data () {
      return {
        mock: {
          'series': [{
            'name': 'gauge',
            'type': 'gauge',
            'detail': {'formatter': '{value}'},
            'data': [{'value': 34}]
          }]
        }
      }
    }
  }
</script>
```
