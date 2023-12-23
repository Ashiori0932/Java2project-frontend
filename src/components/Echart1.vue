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
          text: 'Average Views of Topics', // 设置标题文本内容
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
                  '#6E2C00', // 深棕色
                  '#A04000', // 深橙色
                  '#BA4A00', // 橙红色
                  '#D35400', // 暖橙色
                  '#F39C12', // 橙色
                  '#D4AC0D', // 橙黄色
                  '#F1C40F', // 金黄色
                  '#F4D03F', // 向日葵的黄色
                  '#F7DC6F', // 暖黄色
                  '#F9E79F'  // 淡黄色
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
