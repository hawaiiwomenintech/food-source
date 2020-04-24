<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          🥑 Avocado Prices
        </q-toolbar-title>

        <div>Hawaii Women in Tech v0.0.1</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Data Sources
        </q-item-label>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink'

export default {
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: [
        {
          title: 'Avocado Weights & Nutrition',
          caption: 'usda.gov',
          icon: 'menu_book',
          link: 'https://fdc.nal.usda.gov/fdc-app.html#/food-details/171705/nutrients'
        },
        {
          title: 'USDA ARMS Avocado Retail Report',
          caption: 'marketnews.usda.gov',
          icon: 'bar_chart',
          link: 'https://www.marketnews.usda.gov/mnp/fv-report-retail?commodity=AVOCADOS&repDate=04%2F11%2F2020&repType=&endDate=04%2F17%2F2020&run=&type=retail&startIndex=1&compareLy=No&locChoose=&portal=fv&commodityClass=&region=NATIONAL&region=HAWAII&class=FRUITS&organic=ALL&sortingColumn=avgPrice&sortingOrder=DESC'
        }
      ]
    }
  }
}
</script>
