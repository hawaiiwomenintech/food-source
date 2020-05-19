<template>
  <q-page class="q-pa-md row items-start q-gutter-md bg-grey-2 ">
    <!-- Organic vs non-organic bar chart -->
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6 text-grey-8 text-weight-bolder">
          Organic vs non-organic
        </div>
      </q-card-section>
      <q-card-section class="echarts">
        <IEcharts :option="organicBarChartOption" :resizable="true" />
      </q-card-section>
      <div class="text-subtitle1 text-grey-8 q-pa-md">
        <em>Interesting! <strong>Organic</strong> avocados are cheaper in Hawaii!</em>
      </div>
    </q-card>
    <!-- Hass each bar chart -->
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6 text-grey-8 text-weight-bolder">
          Hass Each Hawaii vs National
        </div>
      </q-card-section>
      <q-card-section class="echarts">
        <IEcharts :option="hassEachBarChartOption" :resizable="true" />
      </q-card-section>
    </q-card>
    <!-- Each Prices -->
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6 text-grey-8 text-weight-bolder">
          Avocado Prices Each
        </div>
      </q-card-section>
      <q-card-section class="q-pa-none echarts">
        <IEcharts :option="eachLineChartOption" :resizable="true" />
      </q-card-section>
      <q-card-section>
        <div class="text-subtitle1 text-grey-8">
          Average avocado weight: <b>0.4 lbs</b>
        </div>
      </q-card-section>
    </q-card>
    <!-- Pound prices -->
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6 text-grey-8 text-weight-bolder">
          Avocado Prices by Pound
        </div>
      </q-card-section>
      <q-card-section class="q-pa-none echarts">
        <IEcharts :option="poundLineChartOption" :resizable="true" />
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import IEcharts from 'vue-echarts-v3/src/full.js'

import hawaiiJson from '../json/hawaii_avocado_prices.json'
const eachPrices = hawaiiJson.each_prices
const poundPrices = hawaiiJson.pound_prices

import natlJson from '../json/national_avocado_prices.json'
const natlEachPrices = natlJson.each_prices
const natlPoundPrices = natlJson.pound_prices

const numeral = require('numeral')

// How to make/style charts: https://echarts.apache.org/en/tutorial.html#ECharts%20Basic%20Concepts%20Overview

export default {
  name: 'charts',
  data () {
    return {
      hassEachBarChartOption: {
        grid: {
          left: '15%',
          bottom: '25%'
        },
        legend: {},
        tooltip: {},
        dataset: {
          dimensions: ['origin', 'each'],
          source: [
            { origin: 'Hawaii', each: hawaiiJson.hass_each_average_price },
            { origin: 'National', each: natlJson.hass_each_average_price }
          ]
        },
        xAxis: {
          type: 'category',
          axisLabel: {
            rotate: 45
          }
        },
        yAxis: {
          axisLabel: {
            formatter: function (value) {
              return numeral(value).format('$0.00').toUpperCase()
            }
          }
        },
        series: [
          { type: 'bar' }
        ]
      },
      organicBarChartOption: {
        grid: {
          left: '15%',
          bottom: '12%'
        },
        legend: {},
        tooltip: {},
        dataset: {
          dimensions: ['origin', 'organic', 'non organic'],
          source: [
            { origin: 'Hawaii', organic: hawaiiJson.organic_each_average_price, 'non organic': hawaiiJson.non_organic_each_price },
            { origin: 'National', organic: natlJson.organic_each_average_price, 'non organic': natlJson.non_organic_each_price }
          ]
        },
        xAxis: {
          type: 'category',
          axisLabel: {
            rotate: 45
          }
        },
        yAxis: {
          axisLabel: {
            formatter: function (value) {
              return numeral(value).format('$0.00').toUpperCase()
            }
          }
        },
        // Declare several bar series, each will be mapped
        // to a column of dataset.source by default.
        series: [
          { type: 'bar' },
          { type: 'bar' }
        ]
      },
      eachLineChartOption: {
        grid: {
          left: '15%',
          bottom: '5%'
        },
        legend: {
          data: ['Hawaii', 'US national']
        },
        tooltip: {},
        xAxis: {
          type: 'category',
          data: eachPrices.map(d => d.month_year)
        },
        yAxis: {
          axisLabel: {
            formatter: function (value) {
              return numeral(value).format('$0.00').toUpperCase()
            }
          }
        },
        // Declare several bar series, each will be mapped
        // to a column of dataset.source by default.
        series: [
          {
            name: 'Hawaii',
            type: 'line',
            data: eachPrices.map(d => d.avg_price),
            smooth: true,
            itemStyle: { color: '#ff851c' }
          },
          {
            name: 'US national',
            type: 'line',
            data: natlEachPrices.map(d => d.avg_price),
            smooth: true,
            itemStyle: { color: '#40ecff' }
          }
        ]
      },
      poundLineChartOption: {
        grid: {
          left: '15%',
          bottom: '25%'
        },
        legend: {
          data: ['Hawaii', 'US national']
        },
        tooltip: {},
        xAxis: {
          type: 'category',
          data: poundPrices.map(d => d.month_year)
        },
        yAxis: {
          axisLabel: {
            formatter: function (value) {
              return numeral(value).format('$0.00').toUpperCase()
            }
          }
        },
        // Declare several bar series, each will be mapped
        // to a column of dataset.source by default.
        series: [
          {
            name: 'Hawaii',
            type: 'line',
            data: poundPrices.map(d => d.avg_price)
          },
          {
            name: 'US national',
            type: 'line',
            data: natlPoundPrices.map(d => d.avg_price)
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
