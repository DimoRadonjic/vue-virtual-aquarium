<script setup>
const props = defineProps({
  fishURL: String,
  selected: Boolean,
});

const model = defineModel();

function selectFish() {
  if (model.value === props.fishURL) {
    model.value = "";
  } else {
    model.value = props.fishURL;
  }
}
</script>

<template>
  <div class="w-32 p-2">
    <img
      :src="`/${props.fishURL}`"
      alt=""
      :class="`object-cover hover:drop-shadow-[0_0_5px_#ff8c42] ${
        selected ? 'drop-shadow-[0_0_10px_#ff8c42]' : ''
      }`"
      @click="selectFish"
    />
  </div>
</template>
