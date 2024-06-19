<template>
  <div class="" v-if="hourlyWeather.length !== 0">
    <div
      class="mx-4 h-fit bg-black/[.05] shadow-[0_0_10px_rgba(255,255,255,0.1)] border-2 border-white/[.1] backdrop-blur p-3 md:m-0 md:p-5 rounded-3xl"
    >
      <h2 class="text-2xl font-bold mb-4 text-white">每小時預報</h2>
      <div class="flex justify-center items-center overflow-hidden relative">
        <div
          class="absolute left-0 top-1/2 transform -translate-y-1/2 cursor-pointer"
          @click="slide('left')"
        >
          <svg
            class="h-8 w-8 text-gray-600"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M15 19l-7-7 7-7"
            ></path>
          </svg>
        </div>

        <div
          class="flex gap-2 md:gap-x-4 transition-transform duration-300 ease-out"
        >
          <div
            v-for="(hourForecast, index) in currentForecasts"
            :key="index"
            class="bg-white rounded-lg p-3 shadow-md flex flex-col items-center md:p-4"
          >
            <div class="text-base font-bold text-[#414141]">
              {{
                new Intl.DateTimeFormat('zh-TW', {
                  hour: '2-digit',
                  minute: 'numeric',
                  weekday: 'short',
                  hourCycle: 'h24',
                  hour12: false,
                }).format(new Date(hourForecast.DateTime))
              }}
            </div>
            <div class="mt-2">
              <img
                :src="hourForecast.WeatherIcon"
                :alt="hourForecast.WeatherIcon"
                class="h-12 w-12"
              />
            </div>
            <div class="text-sm flex">
              <span class="">
                {{ hourForecast.Temperature.Value }}
              </span>
              <span class="">°C </span>
            </div>
            <div class="text-xs text-gray-500">
              {{ hourForecast.IconPhrase }}
            </div>
          </div>
        </div>

        <div
          class="absolute right-0 top-1/2 transform -translate-y-1/2 cursor-pointer"
          @click="slide('right')"
        >
          <svg
            class="h-8 w-8 text-gray-600"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M9 5l7 7-7 7"
            ></path>
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  hourlyWeather: {
    type: Array,
    default: () => [],
  },
  weatherIcon: {
    type: Object,
    default: () => ({}),
  },
})
const hourlyWeather = ref(props.hourlyWeather)
const weatherIcon = ref(props.weatherIcon)

const currentIndex = ref(0)
const numForecasts = hourlyWeather.value.length

const slide = (direction) => {
  if (direction === 'left') {
    currentIndex.value = (currentIndex.value - 1 + numForecasts) % numForecasts
  } else if (direction === 'right') {
    currentIndex.value = (currentIndex.value + 1) % numForecasts
  }
}

const currentForecasts = computed(() => {
  hourlyWeather.value.forEach((item, index) => {
    item.WeatherIcon = weatherIcon.value[item.IconPhrase]
  })
  const start = currentIndex.value
  const end = start + 6
  if (end <= numForecasts) {
    return hourlyWeather.value.slice(start, end)
  } else {
    const remaining = end - numForecasts
    return [
      ...hourlyWeather.value.slice(start),
      ...hourlyWeather.value.slice(0, remaining),
    ]
  }
})
</script>

<style lang="scss" scoped></style>
