<template>
  <main v-if="!loadingLoader">
    <Datatitle :title="title" :todayDate="todayDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries" />

    <div class="mb-20 mt-2" v-if="stats.Country">
      <button
        @click="clearCountryData()"
        class="bg-purple-700 text-white rounded p-3 focus:outline-none hover:bg-purple-800"
      >
        Clear Country
      </button>
    </div>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching Data</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="spinner" />
  </main>
</template>

<script>
import axios from "axios";
import Datatitle from "@/components/DataTitle.vue";
import DataBoxes from "@/components/DataBoxes.vue";
import CountrySelect from "@/components/CountrySelect.vue";

export default {
  name: "Home",
  components: {
    Datatitle,
    DataBoxes,
    CountrySelect,
  },
  data() {
    return {
      loadingLoader: true,
      title: "Global",
      todayDate: "",
      stats: {},
      countries: [],
      loadingImage: require("../assets/spinner.gif"),
    };
  },
  methods: {
    async fetchCovidData() {
      let self = this;
      const res = await axios
        .get("https://api.covid19api.com/summary")
        .then((res) => {
          self.todayDate = res.data.Date;
          self.stats = res.data.Global;
          self.countries = res.data.Countries;
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => (self.loadingLoader = false));
    },
    getCountryData(country) {
      (this.stats = country), (this.title = country.Country);
    },
    async clearCountryData() {
      let self = this;
      self.loadingLoader = true;
      await self.fetchCovidData();
      self.title = "Global";
      self.loadingLoader = false;
    },
  },
  async created() {
    let self = this;
    await self.fetchCovidData();
  },
};
</script>
