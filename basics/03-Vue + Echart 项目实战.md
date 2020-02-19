# Vue + Echart 项目实战

## 安装脚手架
```
yarn global add @vue/cli
```

## 创建项目
```
vue init vue-echarts
```

## 配置vue.config.js

在根目录下创建 vue.config.js：

```js
module.exports = {
  publicPath: '/',
  outputDir: 'dist',
  configureWebpack: {
    devtool: 'source-map',
    externals: {

    }
  },
  devServer: {
    proxy: {
      '/api': {
        target: 'http://localhost:9000',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  }
}
```

## 安装echarts

```
yarn add echarts
```

## 一些模拟数据

##### 柱状图模拟数据 (bar.json)

```json
{
  "data": {
    "name": [
      "衬衫",
      "羊毛衫",
      "雪纺衫",
      "裤子",
      "高跟鞋",
      "袜子"
    ],
    "sales": [
      10,
      30,
      25,
      5,
      40,
      35
    ],
    "cost": [
      5,
      3,
      6,
      0.5,
      4,
      15
    ]
  }
}
```

##### 饼图模拟数据 （pie.json）

```json
{
  "data": {
    "title": "某站点用户访问来源",
    "list": [
      {
        "value": 335,
        "name": "直接访问"
      },
      {
        "value": 310,
        "name": "邮件营销"
      },
      {
        "value": 234,
        "name": "联盟广告"
      },
      {
        "value": 135,
        "name": "视频广告"
      },
      {
        "value": 1548,
        "name": "搜索引擎"
      }
    ]
  }
}
```

## json-server 模拟接口

##### 安装 json-server

```
yarn global add json-server
```

##### 创建json-server运行环境

在项目根目录下创建mock文件夹，将上述的.json模拟数据放进去。再创建一个mock.js文件：

```js
// /mock/mock.js
module.exports = () => {
  return {
    pie: require('./pie.json'),
    bar: require('./bar.json')
  }
}
```

在 package.json 中的 script 中添加脚本：

```json
"mock": "json-server ./mock/mock.js --watch"
```

## App.vue

```vue
<template>
  <div id="app">
    <Pie/>
  </div>
</template>

<script>
import Pie from "./Pie"

export default {
  name: "app",
  components: {
    Pie
  },
  data() {
    return {}
  }
}
</script>

<style lang="scss">
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 600px;
  height: 500px;
}
</style>

```

## Pie.vue

```vue
<template>
  <div ref="container" style="width:500px;height:500px;">
    111
  </div>
</template>

<script>
import echarts from "echarts"

export default {
  name: "app",
  created() {},
  data() {
    return {
      title: "某站点用户访问来源",
      data: []
    }
  },
  components: {},
  methods: {
    initBar() {
      var myChart = echarts.init(this.$refs.container)
      var option = {
        //标题组件，主标题，副标题
        title: {
          text: "echarts 入门",
          link: "https://www.baidu.com",
          subtext: "demo"
        },
        //图例组件
        legend: {
          data: ["销量"],
          show: true,
          right: "20"
        },
        //x坐标轴
        xAxis: {
          data: this.name,
          show: false
        },
        //y坐标轴
        yAxis: {
          show: true,
          show: false
        },
        //系列，通过type指定表的类型
        series: [
          {
            name: "销量",
            type: "pie",
            data: this.data,
            radius: "55%",
            center: ["40%", "50%"]
          }
        ]
      }
      myChart.setOption(option)
    }
  },
  mounted() {
    fetch("http://localhost:3000/data")
      .then(res => res.json())
      .then(res => {
        this.title = res.title
        this.data = res.list
        this.initBar()
      })
  }
}
</script>

<style lang="scss">
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 600px;
  height: 500px;
}
</style>

```

## echarts 其它问题

##### 折线图的圆滑展示，和曲线的差异配置, 文本样式设置

```
smooth
```

##### 如何避免折线数据堆叠的问题？ 

```
stack
```

##### 动画：柱状图动画延迟，animationDelay

```js
 series: [
    {
      name: "销量",
      type: "bar",
      data: this.sales,
      animationDelay: function(idx) {
        return idx * 10
      }
    },
    {
      name: "成本",
      type: "bar",
      data: this.cost,
      animationDelay: function(idx) {
        return idx * 500
      }
    }
  ]
```

```js
animationEasing: "elasticOut",
animationDelayUpdate: function(idx) {
  return idx * 50
}
```

##### 外部数据更新

```js
myChart.setOption(option)
```

## 使用vue-echarts

[前往 vue-echarts 官网](https://github.com/ecomfe/vue-echarts/blob/HEAD/README.zh_CN.md)

##### 安装 vue-echarts

```
yarn add vue-echarts --dev
```

##### 配置 vue.config.js
Vue-ECharts 默认在 webpack 环境下会引入未编译的源码版本，如果你正在使用官方的 Vue CLI 来创建项目，可能会遇到默认配置把 node_modules 中的文件排除在 Babel 转译范围以外的问题。请按如下方法修改配置：

当使用 Vue CLI 3+ 时，需要在 vue.config.js 中的 transpileDependencies 增加 vue-echarts 及 resize-detector，如下：

```js
// vue.config.js
module.exports = {
  transpileDependencies: [
    'vue-echarts',
    'resize-detector'
  ]
}
```

##### 创建 Polar.vue 组件

在 /src/component/Polar.vue

```vue
<template>
  <v-chart :options="polar"/>
</template>

<style>
/**
 * 默认尺寸为 600px×400px，如果想让图表响应尺寸变化，可以像下面这样
 * 把尺寸设为百分比值（同时请记得为容器设置尺寸）。
 */
.echarts {
  width: 100%;
  height: 100%;
}
</style>

<script>
import ECharts from 'vue-echarts'
import 'echarts/lib/chart/line'
import 'echarts/lib/component/polar'

export default {
  components: {
    'v-chart': ECharts
  },
  data () {
    let data = []

    for (let i = 0; i <= 360; i++) {
        let t = i / 180 * Math.PI
        let r = Math.sin(2 * t) * Math.cos(2 * t)
        data.push([r, i])
    }

    return {
      polar: {
        title: {
          text: '极坐标双数值轴'
        },
        legend: {
          data: ['line']
        },
        polar: {
          center: ['50%', '54%']
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross'
          }
        },
        angleAxis: {
          type: 'value',
          startAngle: 0
        },
        radiusAxis: {
          min: 0
        },
        series: [
          {
            coordinateSystem: 'polar',
            name: 'line',
            type: 'line',
            showSymbol: false,
            data: data
          }
        ],
        animationDuration: 2000
      }
    }
  }
}
</script>
```