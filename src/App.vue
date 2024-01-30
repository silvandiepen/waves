<script setup lang="ts">

import { reactive, computed } from 'vue'
import { useBemm } from "bemm";
import InputNumber from "./components/form/InputNumber.vue";
import InputCheckbox from "./components/form/InputCheckbox.vue";

const bemm = useBemm('waves');

const wave = reactive({
  width: 800,
  height: 600,
  offset: 300,
  amplitude: 50,
  phase: 0,
  frequency: 1,
  points: 50,
  inverted: false,
})

const startPath = 'polygon(100% 100%, 0 100% ';
const invertedStartPath = 'polygon(100% 0, 0 0 ';


const units = computed(() => {
  return 2 * Math.PI * wave.frequency / wave.points
})

const generatedWave = computed(() => {
  let clipPathString = wave.inverted ? invertedStartPath : startPath;


  let radPhase = wave.phase * Math.PI / 180;

  for (let i = 0; i <= wave.points; i++) {
    let val = wave.offset + wave.amplitude * Math.cos(i * units.value + radPhase);
    let valY = (val / wave.height * 100).toFixed(2);
    let valX = (i * 100 / wave.points).toFixed(2);
    clipPathString += ', ' + valX + '% ' + valY + '%';
  }
  clipPathString += ');';

  return clipPathString;
})


</script>

<template>
  <div :class="bemm()">
    <div :class="bemm('controls')">

      <InputNumber label="width" v-model="wave.width" />
      <InputNumber label="height" v-model="wave.height" />
      <InputNumber label="offset" v-model="wave.offset" />
      <InputNumber label="amplitude" v-model="wave.amplitude" />
      <InputNumber label="phase" v-model="wave.phase" />
      <InputNumber label="frequency" v-model="wave.frequency" />
      <InputNumber label="points" v-model="wave.points" />

      <InputCheckbox label="Inverted" v-model="wave.inverted" />

    </div>
    <div :class="bemm('container')"
      :style="`--width: ${wave.width}; --height: ${wave.height}; --clip-path: ${generatedWave}`">
      <div :class="bemm('wave')">

      </div>

      <div :class="bemm('code')">
        <pre>clip-path: {{ generatedWave }}</pre>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
.waves {
  display: flex;
  gap: 2em;

  @media screen and (width <= 800px) {
    flex-direction: column;
  }

  &__container {
    padding: 1em;
    border-radius: 1.5em;
    width: fit-content;
    height: fit-content;
    border: 1px solid currentColor;
    gap: 1em;
    display: flex;
    flex-direction: column;

  }

  &__wave {
    background-color: var(--primary);
    border-radius: 1em;
    width: 500px;
    max-width: 80vw;
    clip-path: var(--clip-path);
    aspect-ratio: var(--width) / var(--height);

  }

  &__code {
  
    white-space: break-all;
    max-width: 100%;
    overflow: scroll;
    width: 500px;
    max-width: 80vw;

    border-radius: 1em;      overflow: scroll;

    background-color: black;
    pre {
      width: 100%;
      padding: 2em;

    }
  }
}
</style>
