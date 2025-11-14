<script setup>
import { ref, computed, onMounted, watch } from "vue";

const props = defineProps({
  fish: Object,
});

const emit = defineEmits(["dead"]);

const aquariumEl = ref(null);
const fishEl = ref(null);
const position = ref({ x: 0, y: 0 });
const floatUp = ref(false);
const FLOAT_DELAY = 5000;

const facingLeft = computed(() => {
  return fishEl.value && position.value.x < fishEl.value.offsetLeft;
});

function spawnFish() {
  const aquariumWidth = aquariumEl.value.offsetWidth;
  const aquariumHeight = aquariumEl.value.offsetHeight;

  const fishWidth = fishEl.value.clientWidth;
  const fishHeight = fishEl.value.clientHeight;

  const x = Math.random() * (aquariumWidth - 10 - fishWidth);
  const y = Math.random() * (aquariumHeight - 20 - fishHeight);

  position.value = {
    x: Math.max(0, x),
    y: Math.max(0, y),
  };
}

function moveFish() {
  if (!aquariumEl.value || !fishEl.value || isDead.value) return;

  const aq = aquariumEl.value;
  const fish = fishEl.value;

  const maxX = aq.offsetWidth - fish.clientWidth;
  const maxY = aq.offsetHeight - fish.clientHeight;

  const x = Math.random() * maxX;
  const y = Math.random() * maxY;

  position.value = { x, y };

  const nextMoveDelay = 3000 + Math.random() * 4000;

  setTimeout(moveFish, nextMoveDelay);
}

onMounted(() => {
  aquariumEl.value = document.querySelector("#aquarium");
  spawnFish();

  setTimeout(moveFish, 1000);
});

/* Hunger state */
const hunger = ref(0);
const HUNGRY_THRESHOLD = 70;

const isHungry = computed(() => hunger.value >= HUNGRY_THRESHOLD);

const isDead = computed(() => hunger.value >= 100);

const interval = setInterval(() => {
  if (hunger.value < 100) {
    hunger.value++;
  } else {
    clearInterval(interval);
  }
}, 200);

function feedFish() {
  hunger.value = 0;
}

function removeFish() {
  if (!isDead.value) return;
  emit("dead", props.fish.id);
}

watch(isDead, (dead) => {
  if (dead) {
    const aquariumHeight = aquariumEl.value.offsetHeight;
    const fishHeight = fishEl.value.clientHeight;

    position.value.y = aquariumHeight - fishHeight;

    setTimeout(() => {
      floatUp.value = true;
    }, FLOAT_DELAY);
  }
});

setInterval(() => {
  if (floatUp.value) {
    position.value.y -= 0.4;

    if (position.value.y <= 0) {
      position.value.y = 0;
      floatUp.value = false;
    }
  }
}, 16);
</script>

<template>
  <div
    :class="`${fish.size} p-2  flex flex-col items-center gap-2 fish `"
    ref="fishEl"
    :style="{
      left: position.x + 'px',
      top: position.y + 'px',
      transition: 'left 3s linear, top 3s linear',
    }"
  >
    <button
      v-show="isHungry && !isDead"
      @click="feedFish"
      class="speech-bubble"
    >
      Feed Me <span class="font-bold text-red-600">!</span>
    </button>

    <img
      :src="`/${isDead ? 'dead' : fish.type}.png`"
      :alt="fish.name"
      :style="{
        transform: facingLeft ? 'scaleX(-1)' : 'scaleX(1)',
      }"
      @click="removeFish()"
    />

    <div class="bg-black rounded bg-opacity-80 text-white overflow-hidden">
      <h2 class="p-2 rounded">
        {{ fish.name }}
      </h2>
      <div
        v-show="!isDead"
        class="h-2 bg-red-500 transition-all duration-300 ease-linear"
        :style="{
          width: hunger + '%',
          backgroundColor: `rgb(${hunger * 2.55}, ${255 - hunger * 2.55}, 0)`,
        }"
      ></div>
    </div>
  </div>
</template>

<style scoped>
.fish {
  position: absolute;
}

.speech-bubble {
  @apply absolute bottom-[100%] px-4 py-2 rounded-full left-[50%] whitespace-nowrap;
  background: rgba(255, 255, 255, 0.9);
}
.speech-bubble::before {
  content: "";
  position: absolute;
  border-style: solid;
  border-width: 10px;
  border-color: transparent rgba(255, 255, 255, 0.9) transparent transparent;
  top: 99%;
  left: 50%;
  transform: translateX(-50%) rotate(-90deg);
}
</style>
