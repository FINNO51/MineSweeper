<script setup>
import { ref, reactive } from "vue";

defineProps({
  victory: {
    type: Number,
    required: true,
  },
  counter: {
    type: Number,
    required: true,
  },
  marked: {
    type: Number,
    required: true,
  },
});

function onBeforeEnter(el) {
  setTimeout(() => {}, 500);
}
</script>

<template>
  <div class="score-container">
    <Transition>
      <div class="score" v-if="victory == 0">
        {{ marked + "/" + counter }}
      </div>
      <div
        class="score"
        v-else-if="victory == 1"
        :style="{ color: 'darkgreen' }"
      >
        Victory
      </div>
      <div
        class="score"
        v-else-if="victory == -1"
        :style="{ color: 'darkred' }"
      >
        Defeat
      </div>
    </Transition>
  </div>
</template>

<style scoped>
.score-container {
  display: flex;
  position: relative;
  height: 15vh;
  width: 20vh;
  justify-content: center;
  align-items: center;
}

.score {
  position: absolute;
  border: 0.4vh solid var(--color-border);
  height: 7vh;
  width: 20vh;
  font-size: 5vh;
  font-weight: 700;
  text-align: center;
  justify-content: center;
  line-height: 6vh;
}
</style>
