<template>
  <div>
    <!-- 1.初始化echarts 需要一个有宽高的盒子 -->
    <div ref="mapbox" style="height:600px;width:800px,auto"></div>
  </div>
</template>

<script>
import echarts from "echarts";
// 引入中国地图
import "echarts/map/js/china.js";

//导入jsonp
import jsonp from "jsonp";

// 从echarts实例中复制过来的代码
// const option = {
//     xAxis: {
//         type: 'category',
//         data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
//     },
//     yAxis: {
//         type: 'value'
//     },
//     series: [{
//         data: [820, 932, 901, 934, 1290, 1330, 1320],
//         type: 'line'
//     }]
// };

// 使用地图需要先注册地图
const option = {
  title: {
    text: "中国疫情图",
    link: "#",
    subtext: "作者:hcl",
    sublink: "#"
  },
  series: [
    {
      name: "确诊人数",
      type: "map", //告诉echarts需要渲染一个地图
      map: "china", //告诉echarts需要去渲染中国地图
      label: {
        //控制对应地图的汉字是否显示字体颜色和大小
        show: true,
        color: "#333",
        fontSize: 10
      },
      // 地图板块的颜色
      itemStyle: {
        areaColor: "#eee"
      },
      roam: true,
      zoom: 1.2, //控制地图的放大和缩小
      emphasis: {
        //控制鼠标化过的背景色和字体颜色
        color: "#fff",
        fontSize: 12
      },
      //  itemStyle:{
      //  }
      data: [] //用来展示后台的数据
    }
  ],

  //显示左下角数据段
  visualMap: [
    {
      type: "piecewise",
      show: true,
      pieces: [
        //分段
        { min: 10000 },
        { min: 1000, max: 9999 },
        { min: 100, max: 999 },
        { min: 10, max: 99 },
        { min: 1, max: 9 }
      ],
      // align:'right'默认左边数字
      //orient:'horizontal' 默认竖直
      //right 和 left 控制分段所在位置
      //textStyle:{}
      //各颜色分段
      inRange: {
        symbol: "rect",
        color: ["#ffc0b1", "#9c0505"]
      }
    }
  ],
  //鼠标放在地图上显示数字
  tooltip: {
    trigger: "item"
  },
  toolbox: {
    show: true,
    orient: "vertical",
    left: "right",
    top: "center",
    feature: {
      dataView: { readOnly: false },
      restore: {},
      saveAsImage: {}
    }
  }
};

export default {
  name: "hello",
  // 第二步
  mounted() {
    this.getData();
    this.mychart = echarts.init(this.$refs.mapbox);
    this.mychart.setOption(option);
  },
  methods: {
    getData() {
      jsonp(
        "https://interface.sina.cn/news/wap/fymap2020_data.d.jsonp?_=1580892522427",
        {},
        (err, data) => {
          if (!err) {
            //代表请求数据成功
            console.log(data);
            //处理数据
            let list = data.data.list.map(item => ({
              name: item.name,
              value: item.value
            }));
            option.series[0].data = list;
            this.mychart.setOption(option);
          }
        }
      );
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
