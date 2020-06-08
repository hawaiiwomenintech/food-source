<template>
  <div class="q-pa-md">
    <q-table
      title="Honolulu Avocado Prices"
      :data="data"
      :columns="columns"
      row-key="name"
    />
  </div>
</template>

<script>

const numeral = require('numeral')

export default {
  name: 'AvocadoTables.vue',
  data () {
    return {
      columns: [
        {
          name: 'name',
          label: 'Name',
          align: 'left',
          field: row => row.name,
          format: val => `${val}`,
          sortable: true
        },
        { name: 'price', align: 'center', label: 'Price', field: 'price', format: val => numeral(val).format('$0.00'), sortable: true },
        { name: 'unit', label: 'Unit', field: 'unit' },
        { name: 'store', label: 'Store', field: 'store', sortable: true }
      ],
      data: [
        {
          name: 'Hass Avocados, Each',
          price: 2.79,
          unit: 'each',
          store: 'Foodland'
        },
        {
          name: 'Avocado, Local',
          price: 2.99,
          unit: 'each',
          store: 'Foodland'
        },
        {
          name: 'Avocado, Mesh Bag (4 Count)',
          price: 1.63,
          unit: 'each',
          store: 'Foodland'
        },
        {
          name: 'Organic Avocados, Each',
          price: 2.99,
          unit: 'each',
          store: 'Foodland'
        },
        {
          name: 'Avocado - 4ct Bag',
          price: 1.25,
          unit: 'each',
          store: 'Target'
        },
        {
          name: 'Large Hass Avocado, 1 each',
          price: 2.99,
          unit: 'each',
          store: 'Whole Foods'
        },
        {
          name: 'Medium Hass Avocado, 1 each',
          price: 2.49,
          unit: 'each',
          store: 'Whole Foods'
        },
        {
          name: 'Bagged Hass Avocados',
          price: 1.25,
          unit: 'each',
          store: 'Whole Foods'
        },
        {
          name: 'Organic Medium Hass Avocado, 1 each',
          price: 2.99,
          unit: 'each',
          store: 'Whole Foods'
        },
        {
          name: 'Avocados Hass Large',
          price: 2.50,
          unit: 'each',
          store: 'Safeway'
        },
        {
          name: 'Signature Farms Hass Avocados Prepackaged - 5 count',
          price: 1.20,
          unit: 'each',
          store: 'Safeway'
        },
        {
          name: 'Organic Hass Avocados',
          price: 2.99,
          unit: 'each',
          store: 'Safeway'
        },
        {
          name: 'O Organics Avocados Bagged 4 Ct - 4 Count',
          price: 1.75,
          unit: 'each',
          store: 'Safeway'
        }
      ]
    }
  }
}
</script>

<style scoped>

</style>
