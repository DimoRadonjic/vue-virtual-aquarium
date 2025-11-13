<script setup>
const props = defineProps({
  fish: Object,
  selected: Boolean,
});

const model = defineModel({
  default: {
    name: "Golden Purple Fish",
    type: "golden-purple-fish",
    size: "w-36",
  },
});

function selectFish() {
  if (model.value.name === props.fish.name) {
    model.value = {
      name: "",
      type: "",
      size: "",
    };
  } else {
    model.value = props.fish;
  }
}
</script>

<template>
  <div :class="`w-32 p-2`">
    <img
      :src="`/${props.fish.type}.png`"
      alt=""
      :class="`object-cover  ${
        selected
          ? 'drop-shadow-[0_0_10px_#ff8c42]'
          : 'hover:drop-shadow-[0_0_5px_#ff8c42]'
      }`"
      @click="selectFish"
    />
  </div>
</template>
