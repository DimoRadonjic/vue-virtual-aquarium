<script setup>
import ActiveFish from "./ActiveFish.vue";

defineProps({
  fishes: {
    type: Array,
    required: true,
  },
});

defineEmits(["remove"]);
</script>

<template>
  <div class="aquarium" id="aquarium" ref="fishEl">
    <ActiveFish
      v-for="fish in fishes"
      :key="fish"
      :fish
      @dead="$emit('remove', fish.id)"
    />
  </div>
</template>

<style scoped>
.aquarium {
  position: relative;
  width: 100%;
  background: url("/bg.jpg") no-repeat center center;
  background-size: cover;
  overflow: hidden;
}
</style>
