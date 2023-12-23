<template>
  <div id="echart-wordcloud" :style="{ width: '100%', height: '100%' }"></div>
</template>

<script>
import * as echarts from 'echarts';
import 'echarts-wordcloud';

export default {
  methods: {
    initChart(name, xData, yData) {
      let getChart = echarts.init(document.getElementById('echart-wordcloud'));
      const data = [
        // { name: xData.at(0), value: yData.at(0)},
        // { name: xData.at(1), value: yData.at(1)},
        // { name: xData.at(2), value: yData.at(2)},
        // { name: xData.at(3), value: yData.at(3)},
        // { name: xData.at(4), value: yData.at(4)},
        // { name: xData.at(5), value: yData.at(5)},
        // { name: xData.at(6), value: yData.at(6)},
        // { name: xData.at(7), value: yData.at(7)},
        // { name: xData.at(8), value: yData.at(8)},
        // { name: xData.at(9), value: yData.at(9)},
      ];
      for(let i = 0;i<xData.length;i++){
        const item = {
          name: xData.at(i),
          value: yData.at(i)
        }
        if(!(item.value===0)){
          data.push(item)
        }

      }



      let option = {
        title: {
          text: 'Related Topics', // 设置标题文本内容
          textStyle: {
            color: 'black', // 设置标题文本颜色
            fontSize: 18, // 设置标题文字大小
            fontWeight: 'bold' // 设置标题文字加粗
          },
          top: '93%', // 将标题放置在图表的底部
          left: 'center' // 设置标题水平居中
        },
        tooltip: {},
        series: [{
          name: 'Tag Counts',
          type: 'wordCloud',
          shape: {
            cloudGrow: 0.2
          },
          sizeRange: [40, 100],
          rotationRange: [-30, 30],
          gridSize: 2,
          drawOutOfBound: false,
          layoutAnimation: true,
          keepAspect: true,
          textStyle: {
            fontWeight: 'bold',
            color: function(params) {
              const colors = [
                '#3D2B1F', // 深棕色
                '#573C2B', // 深红棕色
                '#694937', // 深暗棕色
                '#7E5745', // 中等棕色
                '#8F6A53', // 中等暖棕色
                '#A0836B', // 浅暖棕色
                '#B39D8B', // 浅棕色
                '#C1B19F', // 浅灰棕色
                '#D2C9B5', // 浅淡棕色
                '#E4DDCC'  // 极浅棕色
              ];
              return colors[params.dataIndex % colors.length];
            }
          },
          emphasis: {
            textStyle: {
              shadowBlur: 15,
              shadowColor: '#333'
            }
          },
          data: data
          //     data: {
          //       name: xData,
          //       value: yData,
          //     }
        }]
      };
      getChart.setOption(option);

      window.addEventListener("resize", () => {
        getChart.resize();
      });
    },
  },
};
</script>
