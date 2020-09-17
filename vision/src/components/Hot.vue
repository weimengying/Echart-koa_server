g<template>
  <div class="com-container">
    <div class="com-chart" ref="hot_ref"></div>
    <span class="iconfont arr_left" @click="toLeft">&#xe6ef;</span>
    <span class="iconfont arr_right" @click="toRight">&#xe6ed;</span>
    <span class="cat-name"></span>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      chartInstance: null,
      allData: null,
      currentIndex: 0, // 当前所展示出的一级分类数据
      titleFontSize: 0
    }
  },
  mounted() {
    this.initChart()
    this.getData()
    window.addEventListener('resize', this.screenAdapter)
    this.screenAdapter()
  },
  methods: {
    initChart() {
      this.chartInstance = this.$echarts.init(this.$refs.hot_ref, 'chalk')
      const initOption = {
        title: {
          text: '▎热销商品销量金额占比统计',
          left: 20,
          top: 20
        },
        series: [
          {
            type: 'pie'
          }
        ]
      }
      this.chartInstance.setOption(initOption)
    },
    async getData() {
      // 获取服务器的数据，对this.allData进行赋值之后，调用updateChart 方法更新图表
      // const { data: ret } = await this.$http.get('hotproduct')
      const { data: ret } = await axios.get(
        'http://localhost:8999/static/data/hotproduct.json'
      )
      this.allData = ret
      console.log(this.allData)
      this.updateChart()
    },
    updateChart() {
      // 处理图表需要的数据
      const legendData = this.allData[this.currentIndex].children.map(item => {
        return item.name
      })
      const seriesData = this.allData[this.currentIndex].children.map(item => {
        return {
          name: item.name,
          value: item.value
        }
      })
      const dataOption = {
        legend: {
          data: legendData
        },
        series: [
          {
            data: seriesData
          }
        ]
      }
      this.chartInstance.setOption(dataOption)
    },
    screenAdapter() {
      const adapterOption = {}
      this.chartInstance.setOption(adapterOption)
      this.chartInstance.resize()
    },
    toLeft() {
      this.currentIndex--
      if (this.currentIndex < 0) {
        this.currentIndex = this.allData.length
      }
      this.updateChart()
    },
    toRight() {
      this.currentIndex++
      if (this.currentIndex > this.allData.length - 1) {
        this.currentIndex = 0
      }
      this.updateChart()
    }
  }
}
</script>

<style lang="less" scoped>
.arr_left{
    position: absolute;
    left:10%;
    top:50%;
    transform: translateY(-50%);
    cursor: pointer;
    color: #fff;
}
.arr_right{
    position: absolute;
    right:10%;
    top:50%;
    transform: translateY(-50%);
    cursor: pointer;
    color: #fff;
}
</style>
