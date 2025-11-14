<script setup>
import { ref } from "vue";
import FishImage from "./FishImage.vue";

defineProps({
  availableFishes: Array,
});

const emit = defineEmits(["add-fish"]);

const selectedFish = ref(null);
const input = ref(null);

const selectFish = (fish) => {
  selectedFish.value = fish;
  input.value = fish.name;
};

const addFish = () => {
  if (selectedFish.value) {
    const id =
      typeof crypto !== "undefined" && crypto.randomUUID
        ? crypto.randomUUID()
        : `${Date.now()}-${Math.floor(Math.random() * 1e6)}`;
    emit("add-fish", { ...selectedFish.value, id, name: input.value });
    selectedFish.value = null;
    input.value = null;
  }
};
</script>
<template>
  <div
    class="bg-blue-950 w-fit h-full dark:text-white text-xl p-4 flex flex-col gap-10"
  >
    <h1>Fish Type</h1>
    <div class="grid grid-cols-2">
      <FishImage
        v-for="availableFish in availableFishes"
        :fish="availableFish"
        :key="availableFish"
        @click="selectFish(availableFish)"
      />
    </div>
    <form action="" class="flex flex-col gap-6" @submit.prevent="addFish">
      <label for="fishName">Name</label>
      <input
        v-model="input"
        type="text"
        name="fishName"
        id="fishInput"
        class="rounded p-2 text-black"
      />
      <button class="bg-red-700 p-5 rounded hover:bg-red-600">Add Fish</button>
    </form>
  </div>
</template>
<style scoped></style>
