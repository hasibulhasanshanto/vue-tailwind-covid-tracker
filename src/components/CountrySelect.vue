<template>
  <div>
    <div class="mt-10 mb-4">
      <label>Country Tracker</label>
      <select
        @change="countryChange()"
        v-model="selected"
        class="form-select block w-full p-3 rounded border-2"
      >
        <option value="" selected disabled>Please choose a country</option>
        <option
          v-for="country in countries"
          :value="country.ID"
          :key="country.ID"
        >
          {{ country.Country }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: "CountrySelect",
  props: ["countries"],
  data() {
    return {
      selected: "",
    };
  },
  methods: {
    countryChange() {
      const country = this.countries.find((item) => item.ID == this.selected);

      this.$emit("get-country", country);
    },
  },
};
</script>
