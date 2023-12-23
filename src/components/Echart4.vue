<template>
  <div id="echart-line" :style="{width: '100%', height: '100%'}"></div>
</template>

<script>

import * as echarts from 'echarts';

export default {

  methods:{

    initChart(name,xData,yData) {

      let getchart = echarts.init(document.getElementById('echart-line'));
      var  option = {
        tooltip: {
          trigger: 'axis'
        },
        title: {
          text: 'Average Views of Bugs', // 设置标题文本内容
          textStyle: {
            color: 'black', // 设置标题文本颜色
            fontSize: 18, // 设置标题文字大小
            fontWeight: 'bold' // 设置标题文字加粗
          },
          top: '93%', // 将标题放置在图表的底部
          left: 'center' // 设置标题水平居中
        },
        // legend: {
        //   data: [name],
        //   bottom:'0%'
        // },
        grid: { //调整图表上下左右位置
          top:'10%',
          left: '3%',
          right: '8%',
          bottom: '11%',
          containLabel: true
        },

        xAxis: {
          type: 'category',
          boundaryGap: true,
          data: xData,
          axisLabel: {
            color: 'black', // 设置 X 轴标签颜色为绿色
            rotate: -45
          }
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: 'Average Views',
            type: 'bar',
            stack: '总量',
            data: yData,
            itemStyle: {
              // 设置柱子的颜色，可以使用数组指定每个柱子的颜色
              color: function(params) {
                //返回不同的颜色，params.dataIndex 表示柱子在数据中的索引
                const colors = [
                  '#0B0F41', // 深蓝色
                  '#1B1464', // 华丽的蓝色
                  '#30267A', // 深靛色
                  '#2C398C', // 深紫蓝色
                  '#4A4EAF', // 中等蓝色
                  '#5F6EBE', // 浅蓝色
                  '#7D94C1', // 浅灰蓝色
                  '#6FA8DC', // 深浅蓝色
                  '#8AB8F1', // 深深蓝色
                  '#A3B5D8'  // 淡蓝色
                ];
                return colors[params.dataIndex % colors.length];
              }
            }
          }
        ]
      };

      getchart.setOption(option);
      //随着屏幕大小调节图表
      window.addEventListener("resize", () => {
        getchart.resize();
      });
    },

  }

}
</script>
