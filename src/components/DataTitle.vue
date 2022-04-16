<template>
  <div class="text-center">
    <h2 class="text-3xl font-bold">{{ title }}</h2>
    <h6 class="text-2xl mt-4 mb-10">
      {{ timestamp }}
    </h6>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "DataTitle",
  props: ["title", "todayDate"],
  computed: {
    timestamp() {
      return moment(this.todayDate).format("Do MMMM, YYYY, h:mm a");
    },
  },
};
</script>
