
<template>
  <link href="https://fonts.googleapis.com/css?family=Pacifico&display=swap" rel="stylesheet">

  <div class="chart" id="table1" v-if="table1">
    <ChartLine1 ref="chart_line_one"/>
  </div>
  <div class="chart" id="table2" v-if="table2">
    <ChartLine2 ref="chart_line_two"/>
  </div>
  <div class="chart" id="table3" v-if="table3">
    <ChartLine3 ref="chart_line_three"/>
  </div>
  <div class="chart" id="table4" v-if="table4">
    <ChartLine4 ref="chart_line_four"/>
  </div>
  <div class="chart" id="table5" v-if="table5">
    <ChartWordCloud ref="chart_word_cloud"/>
  </div>
  <div class="text">
    <div>
      <img src='http://localhost:5173/src/pictures/7.png' alt="Description of the image">
    </div>
    <div class = "title">
      <p>{{ "Impression of Java" }}</p>
    </div>


    <div class="query" v-if="table5">
      <input type="text" v-model="query" >
      <button id="query" class="query_button" @click="query_theme">Query</button>
    </div>

    <div class="query" v-if="table4">
      <input type="text" v-model="query1" >
      <button id="query1" class="query_button" @click="query_bug">Query</button>
    </div>
    <button id="changeBgBtn" class="last_button" @click="changeBG2">Last Chart</button>
    <button id="changeBgBtn" class="next_button" @click="changeBG1">Next Chart</button>
  </div>
</template>

<script>
import ChartLine1 from './components/Echart1.vue'
import ChartLine2 from './components/Echart2.vue'
import ChartLine3 from './components/Echart3.vue'
import ChartLine4 from './components/Echart4.vue'
import ChartWordCloud from './components/Echart5.vue'
import axios from "axios";

export default{
  data(){
    return{
      name: 'average views',
      xData: ['io', 'stream', 'lambda', 'socket', 'javaFX', 'reflection', 'spring', 'generics', 'exception', 'testing'],
      yData: [30, 132, 80, 134, 100, 120, 80 ,70, 66, 22],
      type: 1,
      table1: true,
      table2: false,
      table3: false,
      table4: false,
      table5: false,
      query: 'spring',
      query1: "RuntimeException"
    }
  },
  mounted () {
    const {name,xData,yData} = this
    if(this.type===1){
      axios.get('http://localhost:8082/question/average-view-count')
          .then(response => {
            // 处理响应数据
            console.log(response.data);
            for (let i = 0; i < 10; i++) {
              this.xData[i]=response.data[i].name
              this.yData[i]=response.data[i].value
            }
            this.$refs.chart_line_one.initChart(name,xData,yData)

          })
          .catch(error => {
            // 处理错误
            console.error('Error fetching data: ', error);
          });
    }
    if(this.type===2){
      this.$refs.chart_line_two.initChart(name,xData,yData)
    }
    if(this.type===3){
      this.$refs.chart_line_three.initChart(name,xData,yData)
    }
    if(this.type===4){
      this.$refs.chart_line_four.initChart(name,xData,yData)
    }
    if(this.type===5){
      this.$refs.chart_word_cloud.initChart(name,xData,yData)
    }
  },
  components: {
    ChartLine1,
    ChartLine2,
    ChartLine3,
    ChartLine4,
    ChartWordCloud
  },
  methods:{
    async changeBG1() {
      //console.log(document.body.style.backgroundImage)
      if (this.type === 5) {
        this.type = 1;
      } else {
        this.type++;
      }

      // axios({
      //   method: 'get',
      //   url: 'http://localhost:8082/question/average-view-count',
      //   headers: {
      //     // 'Content-Type': 'application/json',
      //     // 'Authorization': 'Bearer your_token_here'
      //   }
      // })
      //     .then(response => {
      //       // 处理响应数据
      //       console.log('Response:', response.data);
      //     })
      //     .catch(error => {
      //       // 处理错误
      //       console.error('Error posting data: ', error);
      //     });


      if (this.type === 1) {
        this.table1 = true;
        this.table2 = false;
        this.table3 = false;
        this.table4 = false;
        this.table5 = false;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/1.jpg")';
      } else if (this.type === 2) {
        this.table1 = false;
        this.table2 = true;
        this.table3 = false;
        this.table4 = false;
        this.table5 = false;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/2.jpg")';
      } else if (this.type === 3){
        this.table1 = false;
        this.table2 = false;
        this.table3 = true;
        this.table4 = false;
        this.table5 = false;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/3.jpg")';
      }else if (this.type ===4){
        this.table1 = false;
        this.table2 = false;
        this.table3 = false;
        this.table4 = true;
        this.table5 = false;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/4.jpg")';
      }else{
        this.table1 = false;
        this.table2 = false;
        this.table3 = false;
        this.table4 = false;
        this.table5 = true;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/5.jpg")';
      }

      await this.$nextTick();

      // 根据 type 调用对应的组件方法重新初始化
      if (this.type === 1) {
        axios.get('http://localhost:8082/question/average-view-count')
            .then(response => {
              // 处理响应数据
              console.log(response.data);
              for (let i = 0; i < 10; i++) {
                this.xData[i]=response.data[i].name
                this.yData[i]=response.data[i].value
              }
              this.$refs.chart_line_one.initChart(this.name,this.xData,this.yData)

            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      } else if (this.type === 2) {
        axios.get('http://localhost:8082/question/average-answer-count')
            .then(response => {
              // 处理响应数据
              console.log(response.data);
              for (let i = 0; i < 10; i++) {
                this.xData[i]=response.data[i].name
                this.yData[i]=response.data[i].value
              }
              this.$refs.chart_line_two.initChart(this.name, this.xData, this.yData);

            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      } else if (this.type === 3) {
        axios.get('http://localhost:8082/question/max-view-count')
            .then(response => {
              // 处理响应数据
              console.log(response.data);
              for (let i = 0; i < 10; i++) {
                this.xData[i]=response.data[i].name
                this.yData[i]=response.data[i].value
              }
              this.$refs.chart_line_three.initChart(this.name, this.xData, this.yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      } else if (this.type ===4) {
        axios.get('http://localhost:8082/question/multiple-type-bug-pop')
            .then(response => {
              // 处理响应数据
              console.log(response);
              const name = 'bug-type'
              const xData = [response.data[0].name,response.data[1].name,response.data[2].name,response.data[3].name]
              const yData = [response.data[0].value,response.data[1].value,response.data[2].value,response.data[3].value]
              this.$refs.chart_line_four.initChart(name, xData, yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
        //this.$refs.chart_line_four.initChart(this.name, this.xData, this.yData);
      } else{
        axios.get('http://localhost:8082/question/related-topics', {
          params:{
            input: this.query
          }
        })
            .then(response => {
              // 处理响应数据
              console.log(response);
              const name = "RuntimeException"
              const xData = []
              const yData = []
              for (let i = 0; i < response.data.length; i++) {
                xData.push(response.data[i].name)
                //保留两位
                yData.push(response.data[i].value)
              }
              this.$refs.chart_word_cloud.initChart(name, xData, yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      }

    },
    async changeBG2() {
      //console.log(document.body.style.backgroundImage)
      if (this.type === 1) {
        this.type = 5;
      } else {
        this.type--;
      }

      // axios({
      //   method: 'get',
      //   url: 'http://localhost:8082/question/average-view-count',
      //   headers: {
      //     // 'Content-Type': 'application/json',
      //     // 'Authorization': 'Bearer your_token_here'
      //   }
      // })
      //     .then(response => {
      //       // 处理响应数据
      //       console.log('Response:', response.data);
      //     })
      //     .catch(error => {
      //       // 处理错误
      //       console.error('Error posting data: ', error);
      //     });


      if (this.type === 1) {
        this.table1 = true;
        this.table2 = false;
        this.table3 = false;
        this.table4 = false;
        this.table5 = false;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/1.jpg")';
      } else if (this.type === 2) {
        this.table1 = false;
        this.table2 = true;
        this.table3 = false;
        this.table4 = false;
        this.table5 = false;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/2.jpg")';
      } else if (this.type === 3){
        this.table1 = false;
        this.table2 = false;
        this.table3 = true;
        this.table4 = false;
        this.table5 = false;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/3.jpg")';
      }else if (this.type ===4){
        this.table1 = false;
        this.table2 = false;
        this.table3 = false;
        this.table4 = true;
        this.table5 = false;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/4.jpg")';
      }else{
        this.table1 = false;
        this.table2 = false;
        this.table3 = false;
        this.table4 = false;
        this.table5 = true;
        document.body.style.backgroundImage = 'url("http://localhost:5173/src/pictures/5.jpg")';
      }

      await this.$nextTick();

      // 根据 type 调用对应的组件方法重新初始化
      if (this.type === 1) {
        axios.get('http://localhost:8082/question/average-view-count')
            .then(response => {
              // 处理响应数据
              console.log(response.data);
              for (let i = 0; i < 10; i++) {
                this.xData[i]=response.data[i].name
                this.yData[i]=response.data[i].value
              }
              this.$refs.chart_line_one.initChart(this.name,this.xData,this.yData)

            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      } else if (this.type === 2) {
        axios.get('http://localhost:8082/question/average-answer-count')
            .then(response => {
              // 处理响应数据
              console.log(response.data);
              for (let i = 0; i < 10; i++) {
                this.xData[i]=response.data[i].name
                this.yData[i]=response.data[i].value
              }
              this.$refs.chart_line_two.initChart(this.name, this.xData, this.yData);

            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      } else if (this.type === 3) {
        axios.get('http://localhost:8082/question/max-view-count')
            .then(response => {
              // 处理响应数据
              console.log(response.data);
              for (let i = 0; i < 10; i++) {
                this.xData[i]=response.data[i].name
                this.yData[i]=response.data[i].value
              }
              this.$refs.chart_line_three.initChart(this.name, this.xData, this.yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      } else if (this.type ===4) {
        axios.get('http://localhost:8082/question/multiple-type-bug-pop')
            .then(response => {
              // 处理响应数据
              console.log(response);
              const name = 'bug-type'
              const xData = [response.data[0].name,response.data[1].name,response.data[2].name,response.data[3].name]
              const yData = [response.data[0].value,response.data[1].value,response.data[2].value,response.data[3].value]
              this.$refs.chart_line_four.initChart(name, xData, yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
        //this.$refs.chart_line_four.initChart(this.name, this.xData, this.yData);
      } else{
        axios.get('http://localhost:8082/question/related-topics', {
          params:{
            input: this.query
          }
        })
            .then(response => {
              // 处理响应数据
              console.log(response);
              const name = "RuntimeException"
              const xData = []
              const yData = []
              for (let i = 0; i < response.data.length; i++) {
                xData.push(response.data[i].name)
                //保留两位
                yData.push(response.data[i].value)
              }
              this.$refs.chart_word_cloud.initChart(name, xData, yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      }

    },
    query_theme(){
      axios.get('http://localhost:8082/question/related-topics', {
        params:{
          input: this.query
        }
      })
          .then(response => {
            // 处理响应数据
            console.log(response);
            const name = "RuntimeException"
            const xData = []
            const yData = []
            for (let i = 0; i < response.data.length; i++) {
              xData.push(response.data[i].name)
              //保留两位
              yData.push(response.data[i].value)
            }
            this.$refs.chart_word_cloud.initChart(name, xData, yData);
          })
          .catch(error => {
            // 处理错误
            console.error('Error fetching data: ', error);
          });
    },
    query_bug(){
      if(this.query1.toLowerCase()==="runtimeexception"){
        axios.get('http://localhost:8082/question/single-type-bug-pop', {
          params:{
            bugType: "RuntimeException"
          }
          })
            .then(response => {
              // 处理响应数据
              console.log(response);
              const name = "RuntimeException"
              const xData = []
              const yData = []
              for (let i = 0; i < 10; i++) {
                xData.push(response.data[i].name)
                //保留两位
                yData.push(response.data[i].value)
              }
              this.$refs.chart_line_four.initChart(name, xData, yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      }else if(this.query1.toLowerCase()==="checkedexception"){
        axios.get('http://localhost:8082/question/single-type-bug-pop', {
          params:{
            bugType: "CheckedException"
          }
        })
            .then(response => {
              // 处理响应数据
              console.log(response);
              const name = "RuntimeException"
              const xData = []
              const yData = []
              for (let i = 0; i < 5; i++) {
                xData.push(response.data[i].name)
                //保留两位
                yData.push(response.data[i].value)
              }
              this.$refs.chart_line_four.initChart(name, xData, yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      }else if(this.query1.toLowerCase()==="fatalerror"){
        axios.get('http://localhost:8082/question/single-type-bug-pop', {
          params:{
            bugType: "FatalError"
          }
        })
            .then(response => {
              // 处理响应数据
              console.log(response);
              const name = "FatalError"
              const xData = []
              const yData = []
              for (let i = 0; i < 6; i++) {
                xData.push(response.data[i].name)
                //保留两位
                yData.push(response.data[i].value)
              }
              this.$refs.chart_line_four.initChart(name, xData, yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      }else {
        axios.get('http://localhost:8082/question/multiple-type-bug-pop')
            .then(response => {
              // 处理响应数据
              console.log(response);
              const name = 'bug-type'
              const xData = [response.data[0].name,response.data[1].name,response.data[2].name,response.data[3].name]
              const yData = [response.data[0].value,response.data[1].value,response.data[2].value,response.data[3].value]
              this.$refs.chart_line_four.initChart(name, xData, yData);
            })
            .catch(error => {
              // 处理错误
              console.error('Error fetching data: ', error);
            });
      }

    }

  }
}

</script>
<style>
.chart{
  width: 900px;
  height: 600px;
  opacity: 0.9; /* 设置元素的不透明度为 70% */
  background: white;
  backdrop-filter: blur(5px);
  border-radius: 10px; /* 设置圆角半径为 10 像素 */
  box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.1);
  position: absolute;
  left: 5%;
  top: 50%;
  transform: translate(0, -50%); /* 使用平移将按钮居中 */
  transition: opacity 0.2s ease-in-out;
}
.chart:hover{
  opacity: 1;
  transition: opacity 0.2s ease-in-out;
}
.text{
  width: 420px;
  height: 600px;
  opacity: 0.9; /* 设置元素的不透明度为 70% */
  background: white;
  backdrop-filter: blur(5px);
  border-radius: 10px; /* 设置圆角半径为 10 像素 */
  box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.1);
  position: absolute;
  left: 68%;
  top: 50%;
  transform: translate(0, -50%); /* 使用平移将按钮居中 */
  transition: opacity 0.2s ease-in-out;
}
.text:hover{
  opacity: 1;
  transition: opacity 0.2s ease-in-out;
}
body {
  //background-color: #4A4EAF; /* 设置整个网站的背景颜色 */
  background-image: url('@/pictures/1.jpg');
  background-size: 100% 100%; /* 设置背景图片大小适应屏幕 */
  background-repeat: no-repeat; /* 防止背景图片重复 */
  transition: background-image 0.5s ease-in-out;
}
.next_button {
  padding: 10px 20px; /* 按钮内边距 */
  font-size: 16px; /* 字体大小 */
  border: none; /* 移除边框 */
  border-radius: 5px; /* 圆角 */
  color: #fff; /* 文字颜色 */
  background-color: #444; /* 背景颜色 */
  cursor: pointer; /* 鼠标指针样式 */
  transition: background-color 0.3s ease, box-shadow 0.3s ease; /* 过渡效果 */
  position: absolute; /* 相对定位 */
  top: 88%;
  left: 50%; /* 使按钮水平居中 */
  transform: translate(20%, 0); /* 使用平移将按钮居中 */
  height: 40px;
  width: 120px;
}

.next_button::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.1); /* 透明度为0.1的黑色遮罩 */
  transition: opacity 0.3s ease; /* 透明度过渡效果 */
  border-radius: 5px; /* 与按钮一致的圆角 */
  z-index: -1; /* 放在按钮底部 */
  pointer-events: none; /* 不捕获事件 */
  opacity: 0; /* 初始透明 */
}

.next_button:hover {
  background-color: #333; /* 悬停时的背景颜色 */
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2); /* 悬停时的阴影效果 */
}

.next_button:hover::before {
  opacity: 1; /* 悬停时显示遮罩 */
}

.last_button {
  padding: 10px 20px; /* 按钮内边距 */
  font-size: 16px; /* 字体大小 */
  border: none; /* 移除边框 */
  border-radius: 5px; /* 圆角 */
  color: #fff; /* 文字颜色 */
  background-color: #444; /* 背景颜色 */
  cursor: pointer; /* 鼠标指针样式 */
  transition: background-color 0.3s ease, box-shadow 0.3s ease; /* 过渡效果 */
  position: absolute; /* 相对定位 */
  top: 88%;
  left: 50%; /* 使按钮水平居中 */
  transform: translate(-120%, 0); /* 使用平移将按钮居中 */
  height: 40px;
  width: 120px;
}

.last_button::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.1); /* 透明度为0.1的黑色遮罩 */
  transition: opacity 0.3s ease; /* 透明度过渡效果 */
  border-radius: 5px; /* 与按钮一致的圆角 */
  z-index: -1; /* 放在按钮底部 */
  pointer-events: none; /* 不捕获事件 */
  opacity: 0; /* 初始透明 */
}

.last_button:hover {
  background-color: #333; /* 悬停时的背景颜色 */
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2); /* 悬停时的阴影效果 */
}

.last_button:hover::before {
  opacity: 1; /* 悬停时显示遮罩 */
}
.query_button {
  padding: 10px 20px; /* 按钮内边距 */
  font-size: 16px; /* 字体大小 */
  border: none; /* 移除边框 */
  border-radius: 5px; /* 圆角 */
  color: #fff; /* 文字颜色 */
  background-color: #444; /* 背景颜色 */
  cursor: pointer; /* 鼠标指针样式 */
  transition: background-color 0.3s ease, box-shadow 0.3s ease; /* 过渡效果 */
  height: 40px;
  width: 120px;
  position: absolute;
  left: 64%;
}

.query_button::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.1); /* 透明度为0.1的黑色遮罩 */
  transition: opacity 0.3s ease; /* 透明度过渡效果 */
  border-radius: 5px; /* 与按钮一致的圆角 */
  z-index: -1; /* 放在按钮底部 */
  pointer-events: none; /* 不捕获事件 */
  opacity: 0; /* 初始透明 */
}

.query_button:hover {
  background-color: #333; /* 悬停时的背景颜色 */
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2); /* 悬停时的阴影效果 */
}

.query_button:hover::before {
  opacity: 1; /* 悬停时显示遮罩 */
}

input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  outline: none;
  position: absolute;
  left: 7%;
}

/* 添加悬停样式 */
input:hover {
  border-color: #555;
}

/* 添加焦点样式 */
input:focus {
  border-color: #007bff;
  box-shadow: 0 0 5px #007bff;
}

.query{
  display: flex;
  position: relative; /* 相对定位 */
  top: 75%;
}
img{
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 60%;
  height: auto;
  position: absolute;
  top: 5%;
  left: 50%; /* 使按钮水平居中 */
  transform: translate(-50%, 0);
  opacity: 1;
}
.title{
  font-size: 40px;
  font-family: 'Pacifico', sans-serif;
  position: absolute;
  top: 50%;
  left: 50%; /* 使按钮水平居中 */
  transform: translate(-50%, 0);
  white-space: nowrap;
}
</style>
