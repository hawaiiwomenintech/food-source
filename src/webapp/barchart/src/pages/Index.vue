<template>
  <q-page class="q-pa-md row items-start q-gutter-md bg-grey-2 ">
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6 text-grey-8 text-weight-bolder">
          Local Prices
        </div>
      </q-card-section>
      <q-card-section class="q-pa-none echarts">
        <IEcharts :option="barChartOption" :resizable="true" />
      </q-card-section>
    </q-card>
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6 text-grey-8 text-weight-bolder">
          Prices Over Time
        </div>
      </q-card-section>
      <q-card-section class="q-pa-none echarts">
        <IEcharts :option="lineChartOption" :resizable="true" />
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import IEcharts from 'vue-echarts-v3/src/full.js'
export default {
  name: 'charts',
  data () {
    return {
      barChartOption: {
        grid: {
          bottom: '25%'
        },
        legend: {},
        tooltip: {},
        dataset: {
          dimensions: ['product', '2015', '2016', '2017'],
          source: [
            { product: 'Matcha Latte', 2015: 43.3, 2016: 85.8, 2017: 93.7 },
            { product: 'Milk Tea', 2015: 83.1, 2016: 73.4, 2017: 55.1 },
            { product: 'Cheese Cocoa', 2015: 86.4, 2016: 65.2, 2017: 82.5 },
            { product: 'Walnut Brownie', 2015: 72.4, 2016: 53.9, 2017: 39.1 }
          ]
        },
        xAxis: {
          type: 'category',
          axisLabel: {
            rotate: 45
          }
        },
        yAxis: {},
        // Declare several bar series, each will be mapped
        // to a column of dataset.source by default.
        series: [
          { type: 'bar' },
          { type: 'bar' },
          { type: 'bar' }
        ]
      },
      lineChartOption: {
        grid: {
          bottom: '25%'
        },
        legend: {},
        tooltip: {},
        xAxis: {
          type: 'category',
          data: [
            'Jan', 'Feb', 'Mar', 'Apr', 'May'
          ]
        },
        yAxis: {},
        // Declare several bar series, each will be mapped
        // to a column of dataset.source by default.
        series: [
          {
            type: 'line',
            data: [1, 2, 3, 4, 5]
          }
        ]
      }
    }
  },
  components: {
    IEcharts
  }
}
</script>

<style scoped>
  .echarts {
    width: 400px;
    height: 400px;
  }
  .my-card {
    width: 100%;
    max-width: 400px;
  }
</style>
