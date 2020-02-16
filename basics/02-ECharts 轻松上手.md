# ECharts 轻松上手

```
Echarts：一个纯 Javascript 的图表库，而且ECharts 3 开始不再强制使用 AMD 的方式按需引入，
代码里也不再内置 AMD 加载器。只需要像普通的 JavaScript 库一样用 script 标签引入。

js下载： 
http://echarts.baidu.com/download.html 。(可定制)

ui皮肤定制：
https://echarts.baidu.com/theme-builder/
```

## ECharts 起步

```
<script src="./echarts.min.js"></script>
```

```
在绘图前我们需要为 ECharts 准备一个具备高宽的 DOM 容器
```

```
<div id="container"></div>
```

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>start</title>
  <script src="./echarts.min.js"></script>
  <style>
    #container {
      width: 600px;
      height: 500px;
    }
  </style>
</head>

<body>
  <div id="container">
  </div>

  <script>
    var myChart = echarts.init(document.getElementById('container'));
    var option = {
      //标题组件，主标题，副标题
      title: {
        text: 'echarts 入门',
        link: 'https://www.baidu.com',
        subtext: 'demo'
      },
      //图例组件
      legend: {
        data: ['销量'],
        show: true,
        right: '20'
      },
      //x坐标轴
      xAxis: {
        data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
      },
      //y坐标轴
      yAxis: {
        show: true
      },
      //通过type指定表的类型
      series: [
        {
          name: '销量',
          type: 'bar',
          data: [10, 30, 25, 5, 40, 35],
          animationDelay: function (idx) {
            return idx * 10;
          }
        }
      ]
    }
    myChart.setOption(option);
  </script>

</body>

</html>
```

## 常用配置介绍

```
参见：https://www.echartsjs.com/zh/option.html
```