<template>
  <main v-if="!isLoading" class="mb-10">
    <DataTitle :text="title" :dataDate="dataDate" />

    <DataBoxes :statistics="statistics" />

    <CountrySelect :countries="countries" @get-country="getCountryData"/>

    <button 
      v-if="statistics .Country"
      @click="clearCountryData"
      class="bg-green-700 text-white rounded 
      p-3 mt-10 focus:outline-none hover:bg-green-600"
    >
      Clear Country
    </button>
  </main>
  <main class="flex flex-col align-center justify-center 
  text center" v-else>
   <div class="text-gray-500 text-3xl mt-10 mb-6">
     Fetching Data
   </div>
   <img :src="require('../assets/hourglass.gif')" class="w-24 m-auto" alt="">
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle.vue';
import DataBoxes from '@/components/DataBoxes.vue';
import CountrySelect from '@/components/CountrySelect.vue';

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  data() {
    return {
      isLoading: true,
      title: 'Global',
      dataDate: '',
      statistics: {},
      countries: [],
    }
  },
  methods: {
    async fetchCovidData() {
      const result = await fetch('https://api.covid19api.com/summary');
      const data = await result.json();
      return data;
    },
    getCountryData(country) {
       this.statistics = country;
       this.title = country.Country;
    },
    async clearCountryData() {
      this.isLoading = true;
      const data = await this.fetchCovidData();
      this.title = 'Global';
      this.statistics = data.Global;
      this.isLoading = false;
    }
  },
  async created() {
    const data = await this.fetchCovidData();
    this.dataDate = data.Date;
    this.statistics = data.Global;
    this.countries = data.Countries;
    this.isLoading = false;
  }
}
</script>
