<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { ref, inject, computed } from 'vue';
import BackBtn from './BackBtn.vue';
import ClearButton from './ClearButton.vue';
import CalculateBtn from './calculateBtn.vue';
const accBal = ref(); // account balance
const riskPer = ref(); //risk percentage
const slPips = ref(); // stop loss in pips
// const pipValue = ref(); // pip value in relation to standard, micro and mini lot
const lotSize = ref(''); // lotsize result
const showLot = ref(false); // boolean to toggle
const showLotCalc = inject('showLotCalc');
const header = 'XXX/USD pairs lot size calculator ';
const lotSizeType = ref('Standard');
const lotSizeOptions = [
  { label: 'Standard Lot (1.0)', value: 'Standard' },
  { label: 'Mini Lot (0.1)', value: 'Mini' },
  { label: 'Micro Lot (0.01)', value: 'Micro' },
];
const showw = () => {
  showLot.value = !showLot.value;
};

const pipValue = computed(() => {
  if (lotSizeType.value === 'Standard') return 10; // $10 per pip for 1 lot
  if (lotSizeType.value === 'Mini') return 1; // $1 per pip for 0.1 lot
  if (lotSizeType.value === 'Micro') return 0.1; // $0.10 per pip for 0.01 lot
  return 10; // Default to standard
});

const calc_Lot = () => {
  if (!accBal.value || !riskPer.value || !slPips.value) {
    alert('Please fill in all fields');
    return;
  }
  lotSize.value = ((riskPer.value / 100) * accBal.value) / (slPips.value * pipValue.value);
  showw();
};
const del = () => {
  accBal.value = null;
  slPips.value = null;
  pipValue.value = null;
  riskPer.value = null;
  showLot.value = false;
  lotSizeType.value = 'Standard';
};
</script>

<template>
  <div class="h-screen w-screen">
    <div class="flex items-center justify-center h-full w-full md:p-20">
      <div
        class="md:w-[400px] md:min-h-[400px] min-h-full w-full rounded-xl flex flex-col md:justify-between justify-evenly md:space-y-8 p-1 md:p-6"
      >
        <div>
          <h1 class="w-full text-center font-semibold md:text-3xl text-xl p-1">
            {{ header }}
          </h1>
        </div>
        <p
          class="font-medium text-2xl h-10 text-center p-1"
          :class="{ 'inline-block': showLot, hidden: !showLot }"
        >
          Your lot size is {{ lotSize }}
        </p>
        <div class="flex flex-col space-y-5 p-2">
          <div class="flex w-full md:flex-row flex-col md:space-x-5 space-y-5">
            <div class="flex md:w-1/2 w-full flex-col space-y-2">
              <label :for="accBal">Account Balance</label>
              <input
                type="number"
                v-model="accBal"
                name="accBal"
                placeholder="Enter account balance"
                class="p-3 outline outline-blue-300 rounded-xl"
              />
            </div>
            <div class="flex md:w-1/2 flex-col space-y-2">
              <label :for="riskPer">Risk %</label>
              <input
                type="number"
                v-model="riskPer"
                name="riskPer"
                placeholder="Enter risk percentage"
                class="p-3 outline outline-blue-300 rounded-xl"
              />
            </div>
          </div>
          <div class="flex w-full md:flex-row flex-col md:space-x-5 space-y-5">
            <div class="flex md:w-1/2 w-full flex-col space-y-2">
              <label :for="slPips">Stop loss in pips</label>
              <input
                type="number"
                v-model="slPips"
                name="slPips"
                placeholder="Enter stop loss in pips"
                class="p-3 outline outline-blue-300 rounded-xl"
              />
            </div>
            <div class="flex md:w-1/2 flex-col space-y-2">
              <label :for="pipValue">Pip Value</label>
              <input
                type="number"
                v-model="pipValue"
                name="pipValue"
                placeholder="Enter pip value"
                class="p-3 outline outline-blue-300 rounded-xl"
              />
            </div>
          </div>
          <div class="w-full">
            <label for="lotSizeType" class="mr-2">Select Lot Type:</label>
            <select
              name="lotSizeType"
              id="lotSizeType"
              v-model="lotSizeType"
              class="appearance-auto"
            >
              <option v-for="option in lotSizeOptions" :key="option.label" :value="option.value">
                {{ option.value }}
              </option>
            </select>
          </div>
          <CalculateBtn @click="calc_Lot" />
        </div>
        <ClearButton @click="del" v-if="showLot" />
        <BackBtn @click="showLotCalc" />
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
