<script setup lang="ts">
import { defineAsyncComponent, ref } from 'vue'

const Card = defineAsyncComponent(() => import('@/components/Card.vue'))

const defaultState = () => ({
  1: { content: 'ks', ref: null },
  2: { content: 'kh', ref: null },
  3: { content: 'kd', ref: null }
})

const moveCardNumber = 2
const flipDurationInMs = 300
const elevateDurationInMs = 300
const moveDurationInMs = 300

const cards = ref<Record<number, any>>(defaultState())

function moveCard(card: number) {
  const el = cards.value[card].ref
  if (el) {
    animateFlipCard(card)
    animateElevateCard(card)
    animateMoveCard(card)
  }
}

function animateFlipCard(card: number) {
  cards.value[card].animateFlip = true
}

function animateElevateCard(card: number) {
  setTimeout(() => {
    cards.value[card].animateElevate = true
  }, flipDurationInMs)
}

function animateMoveCard(card: number) {
  setTimeout(() => {
    cards.value[card].animateMove = true
  }, flipDurationInMs + elevateDurationInMs)
}

function resetState() {
  cards.value = defaultState()
}
</script>

<template>
  <div class="container">
    <div class="cards">
      <div class="cards-rows">
        <TransitionGroup name="fade" appear>
          <Card
            v-for="(card, upperKey) in cards"
            :key="`${upperKey}-card`"
            :ref="(el) => (card.ref = el)"
            :content="card.content"
            :flip-duration="flipDurationInMs"
            :class="{
              'card-animate-flip': card.animateFlip,
              'card-animate-elevate': card.animateElevate,
              'card-animate-move': card.animateMove
            }"
          />
        </TransitionGroup>
      </div>
    </div>
    <div class="actions">
      <button @click="moveCard(moveCardNumber)">▶️</button>
      <button @click="resetState">Reset</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  position: relative;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100dvh;
}
.cards {
  position: fixed;
  top: 50%;
}
.cards-rows {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, auto);
  gap: 1rem;
}
.actions {
  margin-top: 1rem;
  display: flex;
  gap: 1rem;
  position: fixed;
  bottom: 1rem;
}
button {
  font-size: 2rem;
  padding: 0.5rem 1rem;
  border: none;
  background-color: #f0f0f0;
  border-radius: 0.5rem;
  filter: drop-shadow(0 0 0.1rem rgba(0, 0, 0, 0.2));
  font-weight: 500;
}
.card-animate-elevate {
  transition: transform v-bind(moveDurationInMs + 'ms');
  transform: perspective(50px) translateZ(5px);
}

.card-animate-move {
  transition: transform v-bind(moveDurationInMs + 'ms');
  transform: translateY(-100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 500ms ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
