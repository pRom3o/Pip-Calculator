<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { ref, provide } from 'vue';
import Lotsize from './Lotsize.vue';
import ClearButton from './ClearButton.vue';
import BackBtn from './BackBtn.vue';
import CalculateBtn from './calculateBtn.vue';

const entryPrice = ref();
const exitPrice = ref();
const pips = ref('');
const showPips = ref(false);

const Pip_calculator = ref(false);
const Lot_calculator = ref(false);
const Header = 'XXX/USD Pairs Pips Calculator';

// home screen
const showPipCalc = () => {
  Pip_calculator.value = !Pip_calculator.value;
};

// toggle for lot calculator screen
const showLotCalc = () => {
  Lot_calculator.value = !Lot_calculator.value;
};

provide('showLotCalc', showLotCalc);

// to display pips value after calculating
const show = () => {
  showPips.value = !showPips.value;
};

// function to calculate pips
const calc_pip = () => {
  pips.value = (Number(entryPrice.value) - Number(exitPrice.value)) * 10000;
  show();
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
        @click="showPipCalc"
        v-if="!Pip_calculator"
        class="flex items-center md:text-3xl text-xl font-bold py-3 space-x-2 cursor-pointer"
        :class="{ hidden: Lot_calculator }"
      >
        <span>Pips calculator |</span>
      </button>
      <!-- <p class="text-3xl" :class="{ hidden: Lot_calculator, hidden: Pip_calculator }">||</p> -->
      <button
        @click="showLotCalc"
        v-if="!Lot_calculator"
        class="flex items-center md:text-3xl text-xl font-bold py-3 space-x-2 cursor-pointer"
        :class="{ hidden: Pip_calculator }"
      >
        <span>| Lot size calculator</span>
      </button>
      <Lotsize v-show="Lot_calculator" />
      <div
        class="md:w-[400px] md:h-[400px] h-full w-full rounded-xl flex flex-col justify-between space-y-10"
        v-show="Pip_calculator"
      >
        <h1 class="w-full text-center font-medium md:text-4xl text-xl">
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
          <CalculateBtn @click="calc_pip(entryPrice, exitPrice)" />
        </div>
        <ClearButton @click="clear" v-if="showPips" />
        <BackBtn @click="showPipCalc" />
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
