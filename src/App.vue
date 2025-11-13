<script setup>
import { ref, watch } from "vue";
import Aquarium from "./Aquarium.vue";
import FishForm from "./FishForm.vue";

const fishes = [
  { name: "Golden Purple Fish", type: "golden-purple-fish", size: "w-36" },
  { name: "Goldfish", type: "goldfish", size: "w-28" },
  { name: "Guppie", type: "guppie", size: "w-24" },
  { name: "Tropical Fish", type: "tropical-fish", size: "w-48" },
  { name: "Tuna", type: "tuna", size: "w-12" },
];

const activeFish = ref([]);

function removeFish(id) {
  activeFish.value = activeFish.value.filter((fish) => fish.id !== id);
}

watch(activeFish.value, (newVal) => {
  console.log("Active fish updated:", newVal);
});
</script>
<template>
  <div class="h-screen w-screen flex border-y-[20px] border-black">
    <FishForm :availableFishes="fishes" @add-fish="activeFish.push($event)" />

    <Aquarium :fishes="activeFish" @remove="removeFish($event)" />
  </div>
</template>
