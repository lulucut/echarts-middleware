# echarts-middleware
在vue中优雅，高效的使用echarts

## 快速上手

### 安装

```
npm install -save echarts
npm install -save echarts-middleware
```

### 引入并注册

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

### 使用
```
<template>
  <div class="my-chart">
    <Chart :options="data"></Chart>
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
### 结果
![结果](http://my-owo-ink.b0.upaiyun.com/puge/github/echarts-middleware/1.png)
