# Vue + Echart 项目实战

基于vue脚手架搭建项目，然后执行：

```
yarn add echarts
```

柱状图模拟数据 (bar.json)

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

饼图模拟数据 （pie.json）

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

App.vue

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

Pie.vue

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

### echarts 其它问题

折线图的圆滑展示，和曲线的差异配置,文本样式设置

```
smooth
```



如何避免折线数据堆叠的问题？ 

```
stack
```

动画：柱状图动画延迟，animationDelay

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

外部数据更新

```js
myChart.setOption(option)
```