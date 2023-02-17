<script setup>
defineProps({
  src: String,
});
</script>

<template>
  <div class="flagContainer">
    <img :src="src" alt="Flag" />
  </div>
</template>

<style lang="scss" scoped>
@use "../style.scss" as *;
img {
  border-radius: 30px;
  background-position: center;
  background-size: cover;
  height: 250px;
}
.flagContainer {
  margin-top: 50px;
  @include center;
}
</style>
