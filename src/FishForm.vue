<script setup>
import { ref } from "vue";
import FishImage from "./FishImage.vue";

defineProps({
  availableFishes: Array,
});

const selectedFish = ref("");
const fishName = ref("");

const addFish = () => {
  console.log("Adding fish:", selectedFish.value);
};
</script>
<template>
  <div
    class="bg-blue-950 w-fit h-full dark:text-white text-xl p-4 flex flex-col gap-10"
  >
    <h1>Fish Type</h1>
    <div class="grid grid-cols-2">
      <FishImage
        v-for="fishName in availableFishes"
        :fishURL="fishName"
        :key="fishName"
        v-model="selectedFish"
        :selected="selectedFish === fishName"
      />
    </div>
    <form action="" class="flex flex-col gap-6">
      <label for="fishName">Name</label>
      <input type="text" name="fishName" id="fishInput" class="rounded p-2" />
      <button
        type="button"
        class="bg-red-700 p-5 rounded hover:bg-red-600"
        @click="addFish"
      >
        Add Fish
      </button>
    </form>
  </div>
</template>
<style scoped></style>
