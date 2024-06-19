<template>
  <div class="" v-if="futureForecasts.length !== 0">
    <div
      class="m-4 bg-black/[.05] shadow-[0_0_10px_rgba(255,255,255,0.1)] border-2 border-white/[.1] backdrop-blur p-5 rounded-3xl md:m-0 md:ml-8"
    >
      <h2 class="text-2xl font-bold mb-4 text-white">未來五天天氣預報</h2>
      <div class="grid grid-cols-2 md:grid-cols-1 gap-4">
        <template
          v-for="(dailyForecast, index) in featureDayWeather"
          :key="index"
        >
          <div
            class="bg-white rounded-lg p-4 shadow-md flex justify-between items-center"
          >
            <div class="md:mr-2">
              <div class="text-base md:text-lg ont-bold">
                {{
                  new Intl.DateTimeFormat('zh-TW', {
                    weekday: 'short',
                  }).format(new Date(dailyForecast.Date))
                }}
              </div>
              <div class="text-[10px] text-gray-500">
                {{
                  new Intl.DateTimeFormat('zh-TW', {
                    month: '2-digit',
                    day: '2-digit',
                  }).format(new Date(dailyForecast.Date))
                }}
              </div>
            </div>

            <div class="flex items-center">
              <div class="mr-1">
                <img
                  :src="dailyForecast.Day.Icon"
                  :alt="dailyForecast.Day.IconPhrase"
                  class="w-8 h-8"
                />
              </div>
            </div>
            <div class="">
              <div class="text-sm">
                {{ dailyForecast.Temperature.Maximum.Value }}°C
              </div>
            </div>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, watch } from 'vue'

const props = defineProps({
  futureForecasts: {
    type: Array,
    default: () => [],
  },
  weatherIcon: {
    type: Object,
    default: () => ({}),
  },
})
const futureForecasts = ref(props.futureForecasts)
const weatherIcon = ref(props.weatherIcon)

const featureDayWeather = computed(() => {
  let data = []
  futureForecasts.value.forEach((item, index) => {
    item.Day.Icon = weatherIcon.value[item.Day.IconPhrase]
    data.push(item)
  })
  return data
})

watch(
  () => props.futureForecasts,
  (newData) => {
    futureForecasts.value = newData
  },
  { deep: true }
)
</script>

<style lang="scss" scoped></style>
