<script setup>
import ActiveFish from "./ActiveFish.vue";
</script>

<template>
  <div class="aquarium w-full h-full">
    <ActiveFish />
  </div>
</template>

<style scoped>
.aquarium {
  background-image: url("/bg.jpg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
</style>
