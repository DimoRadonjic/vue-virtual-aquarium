<script setup>
import Aquarium from "./Aquarium.vue";
import FishForm from "./FishForm.vue";

const fishes = [
  "goldfish.png",
  "tuna.png",
  "guppie.png",
  "golden-purple-fish.png",
  "tropical-fish.png",
];
</script>
<template>
  <div class="h-screen w-screen flex border-y-[20px] border-black">
    <FishForm :availableFishes="fishes" />
    <Aquarium />
  </div>
</template>
