<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { ref } from 'vue';

const entryPrice = ref();
const exitPrice = ref();
const pips = ref([]);
const showPips = ref(false);

const Pip_calculator = ref(false);
const Header = 'EURUSD Pips Calculator';

// home screen
const showCalc = () => {
  Pip_calculator.value = !Pip_calculator.value;
};

// to display pips value after calculating
const show = () => {
  showPips.value = !showPips.value;
};

// function to calculate pips
const calc_pip = () => {
  pips.value = Math.round((Number(entryPrice.value) - Number(exitPrice.value)) * 10000);
  show();
  console.log(Number(pips.value));
};

// function to reset inputs and remove calculated pip value from display
const clear = () => {
  entryPrice.value = null;
  exitPrice.value = null;
  showPips.value = false;
};
</script>

<template>
  <div class="w-screen h-screen font-sans">
    <div class="flex items-center justify-center h-full w-full md:p-20 p-5">
      <button
        @click="showCalc"
        v-if="!Pip_calculator"
        class="flex items-center text-3xl font-bold p-3 space-x-3 hover:border-b border-b-gray-400 cursor-pointer"
      >
        <span>Pips Calculator</span
        ><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
          <path
            fill="none"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M5 12h14m-6 6l6-6m-6-6l6 6"
          />
        </svg>
      </button>
      <div
        class="md:w-[400px] md:h-[400px] h-full w-full rounded-xl flex flex-col justify-between space-y-10"
        v-show="Pip_calculator"
      >
        <h1 class="w-full text-center font-medium md:text-4xl text-2xl p-8">
          {{ Header }}
        </h1>
        <p
          class="font-medium text-2xl h-10 text-center p-1"
          :class="{ 'inline-block': showPips, hidden: !showPips }"
        >
          Your target is {{ pips }} Pips
        </p>
        <div class="space-y-10 p-2">
          <div class="space-y-2 flex flex-col">
            <label :for="entryPrice" class="">Entry Price</label>
            <input
              type="number"
              name="entryPrice"
              v-model="entryPrice"
              placeholder="Entry Price"
              class="p-3 outline outline-blue-300 rounded-xl"
            />
          </div>
          <div class="space-y-2 flex flex-col">
            <label :for="exitPrice" class="">Exit Price</label>
            <input
              type="number"
              name="exitPrice"
              v-model="exitPrice"
              placeholder="Exit Price"
              class="p-3 outline outline-blue-300 rounded-xl"
            />
          </div>
          <button
            @click="calc_pip(entryPrice, exitPrice)"
            class="p-2 bg-blue-400 w-full rounded-xl cursor-pointer hover:bg-blue-300"
          >
            Calculate pips
          </button>
          <button
            @click="clear"
            class="w-full flex items-center justify-center p-2 space-x-1 bg-red-300 rounded-xl cursor-pointer hover:bg-red-200"
          >
            <span>Clear</span>
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24">
              <path
                fill="none"
                stroke="#b80707"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M4 7h16m-10 4v6m4-6v6M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2l1-12M9 7V4a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v3"
              />
            </svg>
          </button>
        </div>

        <button
          @click="showCalc"
          class="w-full flex items-center justify-center p-3 space-x-5 bg-gray-100 rounded-xl hover:bg-gray-200 cursor-pointer"
        >
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="18" viewBox="0 0 24 24">
            <path
              fill="none"
              stroke="#b80707"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M5 12h14M5 12l6 6m-6-6l6-6"
            /></svg
          >Back
        </button>
      </div>
    </div>
  </div>
</template>
<style scoped>
/* Hide number input arrows for Chrome, Edge, Safari */
input[type='number']::-webkit-inner-spin-button,
input[type='number']::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
