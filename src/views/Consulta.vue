<template>
  <div class="container" id="app">
  <v-card>
    <v-progress-linear v-slot:progress color="blue" ></v-progress-linear>
    <v-card-title>
      Investment Stock to Buy
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="search"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
    v-model="selected"
    :headers="headers"
    :items="stocks"
    item-key="name"
    select-all
    :search="search"
    class="elevation-1"

    >

      <template v-slot:items="props">
      <td>
        <v-checkbox
        v-model="props.selected"
        primary
        hide-details
        ></v-checkbox>
      </td>
      <td>{{ props.item.name }}</td>

      <td class="text-xs-right">{{ props.item.name }}</td>
      <td class="text-xs-right">{{ props.item.value.start }}</td>
      <td class="text-xs-right">{{ props.item.value.end }}</td>
      <td class="text-xs-right">{{ props.item.date }}</td>
      </template>
      <template v-slot:no-results>
        <v-alert :value="true" color="error" icon="warning">
          Your search for "{{ search }}" found no results.
        </v-alert>
      </template>

    </v-data-table>

</v-card>
  </div>

</template>
<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';
import Axios from 'axios';

export default {
  data() {
    return {

      search: '',
      selected: [],

      headers: [
        {
          text: 'Stocks (by most popular) ',
          align: 'left',
          sortable: false,
          value: 'name',
        },
        { text: 'Stock', value: 'name', align: 'right' },
        { text: 'Start', value: 'start', align: 'right' },
        { text: 'End', value: 'end', align: 'right' },
        { text: 'Period', value: 'period', align: 'right' },
      ],

      stock:
      {
        id: 0,
        name: '',
        start: 0,
        end: 0,
        period: Date,
      },

      stocks: [],

    };
  },
  mounted() {
    Axios
      .get('https://damp-bayou-25011.herokuapp.com/api/v1/stock')
      .then(response => (this.stocks = response.data));
  },


};


</script>
<style lang="sass" scoped>

</style>
