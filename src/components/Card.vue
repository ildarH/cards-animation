<script setup lang="ts">
import { computed, ref } from 'vue'
import ks from '@/assets/ks.svg'
import kd from '@/assets/kd.svg'
import kh from '@/assets/kh.svg'

interface IProps {
  content: string
  flipDuration: number
}

const props = withDefaults(defineProps<IProps>(), {
  content: 'placeholder',
  flipDuration: 300
})

const card = ref(null)

const currentCardImage = computed(() => {
  return {
    ks,
    kh,
    kd
  }[props.content]
})

defineExpose({ card })
</script>

<template>
  <div v-bind="$attrs" ref="card" class="card-wrapper">
    <div class="card">
      <div class="card__front">
        <img src="@/assets/back.png" alt="back" />
      </div>
      <div class="card__back">
        <img :src="currentCardImage" alt="front" />
      </div>
    </div>
  </div>
</template>

<style scoped>
img {
  width: 100%;
  height: 100%;
}
.card-wrapper {
  width: 14rem;
  height: 20rem;
  perspective: 60rem;
  filter: drop-shadow(0 0rem 0.3rem rgba(0, 0, 0, 0.5));
}

.card {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform v-bind(flipDuration + 'ms');
  transform-style: preserve-3d;
}

.card-animate-flip .card {
  transform: rotateY(180deg);
}
.card__front,
.card__back {
  position: absolute;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden; /* Safari */
  backface-visibility: hidden;
}
.card__back {
  transform: rotateY(180deg);
}
</style>
