<script setup>
import { ref } from "vue";

const props = defineProps({
  fishURL: String,
});

const selectedFish = defineModel("selectedFish", { default: "" });

const selected = ref(false);

function selectFish() {
  selectedFish.value = props.fishURL;
  selected.value = !selected.value;
}
</script>

<template>
  <div class="w-32 p-2">
    <img
      :src="`/${fishURL}`"
      alt=""
      :class="`object-cover hover:drop-shadow-[0_0_5px_#ff8c42] ${selected ? 'drop-shadow-[0_0_10px_#ff8c42]' : ''}`"
      @click="selectFish()"
    />
  </div>
</template>

<style scoped></style>
