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
          text: 'Average Answers of Topics', // 设置标题文本内容
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
            name: 'Average Answers',
            type: 'bar',
            stack: '总量',
            data: yData,
            itemStyle: {
              // 设置柱子的颜色，可以使用数组指定每个柱子的颜色
              color: function(params) {
                const colors = [
                  '#0F261C', // 深绿色
                  '#1D3C2B', // 深翠绿色
                  '#336650', // 深碧绿色
                  '#4C7B73', // 深青绿色
                  '#66988A', // 中等绿色
                  '#80B3A1', // 浅绿色
                  '#9BC2B6', // 浅青绿色
                  '#B5D1CB', // 浅碧绿色
                  '#C5DAD1', // 极浅绿色
                  '#D5E0D7'  // 极极浅绿色
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
