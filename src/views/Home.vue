<template>
  <main v-if="!loadingLoader">
    <Datatitle :title="title" :todayDate="todayDate" />
    <DataBoxes :stats="stats" />
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

export default {
  name: "Home",
  components: {
    Datatitle,
    DataBoxes,
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
          console.log(res.data);
          self.todayDate = res.data.Date;
          self.stats = res.data.Global;
          self.countries = res.data.Countries;
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => (self.loadingLoader = false));
    },
  },
  async created() {
    let self = this;
    console.log("Created");
    await self.fetchCovidData();
  },
};
</script>
