<template>
  <div class="box page-wrapper">
    <SubscribtionChart></SubscribtionChart>
  </div>
</template>

<script>
import SubscribtionChart from "./components/SubscribtionChart.vue";

export default {
  name: "app",
  components: {
    SubscribtionChart,
  },
};
</script>
<style lang="css">
* {
  font-family: Arial, Helvetica, sans-serif;
}
.page-wrapper {
  width: 500px;
  height: 300px;
  margin: auto;
}
</style>