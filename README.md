# echarts-middleware
在vue中优雅，高效的使用echarts,不断完善中

## 快速上手

安装

```
npm install -save echarts
npm install -save echarts-middleware
```

引入并注册

```
<script>
  import 'echarts'
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
    <Chart :opt="data" :w="400" :h="400"></Chart>
  </div>
</template>

<script>
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

## 组件特点
* 高效: 直接将echarts实例暴露给vue,可以直接操作
* 优雅: 自动处理echarts的 注册 及 销毁 操作
* 简洁: 主要代码少于50行 没有性能损失
* 接口丰富: 由于直接暴露 echarts实例 所以理论上能用 100% echarts方法

## 简单对比

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

## 参数

| 参数        | 含义         | 类型  | 是否必须  |
| ----------- |:-------------:| -----:| -----:|
|opt| 图表的配置 | Object | true |
|w| 图表的宽度| Number | false |
|h| 图表的高度| Number | false |
|theme| 主题| String, Object | false |
|renderer| 渲染器| String | false |

使用实例：

```
<Chart :opt="data" :w="400" :h="400"></Chart>
```

注:如果 w h 参数没有设置，组件会以父组件宽高作为组件宽高， 如果父组件没有宽高则会以400px作为宽高。



## 获取Echarts对象的方法

1.使用refs取得Echarts实例
```
<template>
  <div>
    <Chart ref="mychart" :opt="data" :w="400" :h="400"></Chart>
    <button @click="click">销毁图表</button>
  </div>
</template>
<script>
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
    },
    methods: {
      click () {
        const chart = this.$refs['mychart'].chart // 有的版本需要用this.$refs['mychart'][0].chart
        console.log('输出echarts对象', chart)
        console.log('销毁echarts图表')
        chart.dispose()
      }
    }
  }
</script>
```
2.使用v-model接收echarts实例
```
<template>
  <div>
    <Chart v-model="chart" :opt="data" ref="mychart" :w="400" :h="400"></Chart>
    <button @click="click">销毁图表</button>
  </div>
</template>

<script>
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
        },
        chart: null
      }
    },
    methods: {
      click () {
        const chart = this.chart
        console.log('输出echarts对象', chart)
        console.log('销毁echarts图表')
        chart.dispose()
      }
    }
  }
</script>
```
支持的方法，理论支持echarts所有方法，具体方法列表请查阅 [API文档](http://echarts.baidu.com/api.html#echarts)

举例:使用`this.$refs[ref名][0].chart.dispose()` 销毁图表

<<<<<<< HEAD
## 常见问题
1. 图表不刷新？ 解决办法： 使用 :key

2.错误问题: __DEV__ is not defined
  解决方法:
  ```
  在 webpack的配置里面加一个配置

  plugins: [
    new webpack.DefinePlugin({
      __DEV__: false
    })
  ]
  ```
