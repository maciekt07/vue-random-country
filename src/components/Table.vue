<script setup>
const numberWithSpaces = (x) =>
  x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
defineProps({
  capital: String,
  population: Number,
  area: Number,
  region: String,
});
</script>
<template>
  <div class="container">
    <div class="table">
      <b>Capital:</b> {{ capital }}<br />
      <b>Population:</b> {{ numberWithSpaces(population) }}<br />
      <b>Area:</b> {{ numberWithSpaces(area) }} km²<br />
      <b>Region:</b> {{ region }}
    </div>
  </div>
</template>
<style lang="scss" scoped>
@use "../style.scss" as *;

.container {
  @include center();
}
.table {
  color: white;
  background: rgba(255, 255, 255, 0.173);
  backdrop-filter: blur(20px);
  padding: 15px;
  border-radius: 15px;
  width: auto;
}
</style>
