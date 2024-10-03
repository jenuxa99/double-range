<script setup>
import { ref, reactive } from 'vue';

const yearList = reactive([2017, 2018, 2019, 2020, 2021, 2022, 2023]);
let monthList = reactive(["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec",]);
let dynamicYearList = reactive([]);

outerLoop: for (const year of yearList) {
  for (const month of monthList) {
    if (year === 2023 && month === "Feb") {
      break outerLoop;
    }
    dynamicYearList.push([year, month]);
  }
}

const startMainValue = ref(yearList[0]);
const endMainValue = ref(yearList[yearList.length - 1]);

const startSecValue = ref(0);
const endSecValue = ref(dynamicYearList.length - 1);




const updateStartMainValue = (event) => {
  startMainValue.value = Number(event.target.value);

  if (startMainValue.value >= endMainValue.value) {
    startMainValue.value = endMainValue.value - 1;
  }

  if (startMainValue.value > dynamicYearList[0][0]) {
    dynamicYearList.splice(0, 12)
    endSecValue.value = dynamicYearList.length - 1;
  }

  if (startMainValue.value < dynamicYearList[0][0]) {
    monthList.slice().reverse().forEach((month) => {
      dynamicYearList.unshift([startMainValue.value, month])
      endSecValue.value = dynamicYearList.length - 1;
    })
  };

}

const updateEndMainValue = (event) => {
  endMainValue.value = Number(event.target.value);

  if (endMainValue.value <= startMainValue.value) {
    endMainValue.value = startMainValue.value + 1;
  }

  if (endMainValue.value < dynamicYearList[dynamicYearList.length - 1][0]) {
    dynamicYearList.splice([dynamicYearList.length - 12], 12)
    endSecValue.value = dynamicYearList.length - 1;
  }
  if (endMainValue.value > dynamicYearList[dynamicYearList.length - 1][0]) {
    monthList.forEach((month) => {
      dynamicYearList.push([endMainValue.value, month])
      endSecValue.value = dynamicYearList.length - 1;
    })
  };
};

const updateStartSecValue = (event) => {
  startSecValue.value = Number(event.target.value);

  if (startSecValue.value >= endSecValue.value) {
    startSecValue.value = endSecValue.value - 1;
  }
};

const updateEndSecValue = (event) => {
  endSecValue.value = Number(event.target.value);

  if (endSecValue.value <= startSecValue.value) {
    endSecValue.value = startSecValue.value + 1;
  }
};
</script>

<template>
  <div>
    <section class="my-20 mx-20">
      <div class="relative w-full">
        <input type="range" :min="yearList[0]" :max="yearList[yearList.length - 1]" :value="startMainValue"
          class="absolute w-full slider" @input="updateStartMainValue($event)" />
        <input type="range" :min="yearList[0]" :max="yearList[yearList.length - 1]" :value="endMainValue"
          class="absolute slider w-full" @input="updateEndMainValue($event)" />
      </div>
      <div
        class="flex justify-between w-full h-20 pt-4 bg-red-300 rounded-b-xl border border-black border-t-transparent">
        <span class="" v-for="(year, index) in yearList" :key="index">{{ year }}</span>
      </div>
      <div>
        <p class="mt-4 text-center">Thumb-min: {{ startMainValue }}</p>
        <p class="mt-4 text-center">Thumb-max: {{ endMainValue }}</p>
      </div>
    </section>

    <section class="flex flex-col gap-1 my-20 mx-20">
      <div class="relative w-full">
        <input type="range" :min="0" :max="dynamicYearList.length - 1" :value="startSecValue"
          class="absolute slider w-full" @input="updateStartSecValue($event)" />
        <input type="range" :min="0" :max="dynamicYearList.length - 1" :value="endSecValue"
          class="absolute slider w-full" @input="updateEndSecValue($event)" />
      </div>
      <div
        class="flex justify-between w-full px-4 pt-3 h-20 bg-red-300 rounded-b-xl border border-black border-t-transparent overflow-hidden">
        <div v-for="(item, index) in dynamicYearList" :key="index" class="flex flex-col justify-between py-2">
          <span class="opacity-0 text-[1px]" :class="{ 'opacity-100 text-[15px]': (index + 1) % 12 === 1 }">{{ item[0]
            }}</span>
          <span class="rotate-45 mt-2 text-[14px]">{{ item[1] }}</span>
        </div>
      </div>
      <div>
        <p class="mt-4 text-center">Thumb-min-year: {{ dynamicYearList[startSecValue][0] }}</p>
        <p class="mt-4 text-center">Thumb-min-month: {{ dynamicYearList[startSecValue][1] }}</p>
        <p class="mt-4 text-center">Thumb-max-year: {{ dynamicYearList[endSecValue][0] }}</p>
        <p class="mt-4 text-center">Thumb-max-month: {{ dynamicYearList[endSecValue][1] }}</p>

      </div>
    </section>
  </div>
</template>

<style scoped>
.slider {
  -webkit-appearance: none;
  appearance: none;
  outline: none;
  width: 100%;
  height: 4px;
  background: black;
}

.slider::-moz-range-thumb {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: brown;
  cursor: pointer;
}

.opacity-100-12:nth-child(12n+1) {
  @apply opacity-100;
}
</style>
