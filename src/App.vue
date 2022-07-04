<template>
  <div id="app">
    <b-container>
      <b-card class="text-center mb-2">
        <b-table  hover :items="listOfCity" :fields="fields">
          <template #cell(city_ascii)="row">
            <b-button size="sm" @click="listOfCity.splice(row.index, 1)" class="mr-2"> Delete</b-button>
          </template>
          <template #cell(time)="row">
            <span>{{ displayTimeInTimeZone(row.item.timezone) }}</span>
          </template>
        </b-table>
      </b-card>
      <b-card class="text-center mt-5" title="Search for Time Zone">
        <b-form inline @submit="getTimeZoneViaCity">
          <label class="sr-only" for="inline-form-input-name">City Name</label>
          <b-form-input
            id="inline-form-input-name"
            v-model="cityName"
            class="mb-2 mr-sm-2 mb-sm-0"
            placeholder="City Name"
          ></b-form-input>
          <b-button variant="primary" type="submit">Search</b-button>
        </b-form>
      </b-card>
      <b-card class="text-center mt-3" v-show="filteredCity && filteredCity.length > 0" title="Search Result..">
        <b-card-text>
          <b-table   hover :items="filteredCity" :fields="fields">
            <template #cell(city_ascii)="row">
              <b-button size="sm" @click="filteredCity.splice(row.index, 1)" class="mr-2"> Delete</b-button>
            </template>
            <template #cell(time)="row">
            <span>{{ displayTimeInTimeZone(row.item.timezone) }}</span>
          </template>
          </b-table>
        </b-card-text>
        <b-link @click="addToTable" class="card-link">Add to list</b-link>
      </b-card>
    </b-container>
  </div>
</template>

<script>
import { timeZomeData } from './timeZone';
import moment from 'moment-timezone';
export default {
  name: 'App',
  data() {
    return {
      fields: [
        {
          key: 'city',
          sortable: true,
        },
        {
          key: 'country',
          sortable: false,
        },
        {
          key: 'timezone',
          sortable: true,
        },
        {
          key: 'time',
        },
        {
          key: 'city_ascii',
          sortable: false,
          label: 'Action',
        },
      ],
      cityName: '',
      listOfCity: [],
      filteredCity: [],
    };
  },
  methods: {
    getTimeZoneViaCity() {
      event.preventDefault();
      this.filteredCity =
        timeZomeData.filter((item) => {
          if (item && item.city) {
            return item.city.toLocaleLowerCase() === this.cityName.toLocaleLowerCase();
          }
        }) || [];
    },
    addToTable() {
      if (this.filteredCity && this.filteredCity.length > 0) {
        this.cityName = '';
        const uniqueCities = [...this.listOfCity, ...this.filteredCity];
        this.listOfCity = [...new Set(uniqueCities)];
        this.filteredCity = [];
      }
    },
    displayTimeInTimeZone(timeZone = 'Asia/Karachi') {
      return moment.tz(moment(), timeZone).format('HH:mm');
    },
  },
};
</script>
