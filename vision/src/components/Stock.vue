<template>
  <div class="com-container">
    Stock组件
    <div class="com-chart" ref="stock_ref"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      chartInstance: null,
      allData: null
    }
  },
  mounted (){
      this.initChart()
      this.getData()
      window.addEventListener('resize',)
  },
  methods: {
    initChart() {
      this.echartInstance = this.$echarts.init(this.$refs.stock_ref)
      const initOption = {}
      this.echartInstance.setOption(initOption)
    }
  }
}
</script>
<style>
</style>
