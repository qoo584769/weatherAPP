<template>
  <div class="" v-if="Object.keys(currentWeather).length !== 0">
    <div
      class="m-4 bg-black/[.05] shadow-[0_0_10px_rgba(255,255,255,0.1)] border-2 border-white/[.1] backdrop-blur text-white py-5 md:p-5 md:m-0 md:mb-10 rounded-3xl"
    >
      <div class="text-5xl mb-4">
        {{ currentWeather?.WeatherElement?.AirTemperature }} °C
      </div>

      <div
        class="text-3xl font-semibold mb-2 md:mb-4 flex items-center justify-center"
      >
        <span class="pr-4 border-r-[1px] border-white/[.5]">
          {{ currentWeather?.GeoInfo.CountyName }}
        </span>
        <img
          :src="
            weatherIcon[currentWeather?.WeatherElement?.Weather] || overcast
          "
          alt=""
          class="w-12 h-12 ml-2 mr-1"
        />
        <span class="text-xl font-medium text-white/[.8]">
          {{ currentWeather?.WeatherElement?.Weather }}
        </span>
      </div>

      <div class="mb-2 md:mb-4">
        <span class="p-4"
          >H :
          {{
            currentWeather.WeatherElement.DailyExtreme.DailyHigh
              ?.TemperatureInfo?.AirTemperature
          }}
          °C</span
        >
        <span class="p-4"
          >L :
          {{
            currentWeather.WeatherElement.DailyExtreme.DailyLow?.TemperatureInfo
              ?.AirTemperature
          }}
          °C</span
        >
      </div>

      <div
        class="flex md:gap-4 flex-wrap justify-evenly md:justify-around text-base md:text-lg"
      >
        <div class="flex flex-col items-center">
          <span class="">紫外線</span>
          <span class="">
            {{
              currentWeather.WeatherElement.UVIndex !== undefined
                ? currentWeather.WeatherElement.UVIndex
                : '無資料'
            }}
            <span class="">{{
              currentWeather.WeatherElement.UVIndex !== undefined ? 'UVI' : ''
            }}</span>
          </span>
        </div>
        <div class="border-r"></div>
        <div class="flex flex-col items-center">
          <span class="">風速</span>
          <span class=""
            >{{ currentWeather.WeatherElement.WindSpeed }} m/s</span
          >
        </div>
        <div class="border-r"></div>
        <div class="flex flex-col items-center">
          <span class="">濕度</span>
          <span class=""
            >{{ currentWeather.WeatherElement.RelativeHumidity }} %</span
          >
        </div>
        <div class="border-r"></div>
        <div class="flex flex-col items-center">
          <span class="">能見度</span>
          <span class=""
            >{{
              currentWeather.WeatherElement.VisibilityDescription || '無資料'
            }}
            <span class="">{{
              currentWeather.WeatherElement.VisibilityDescription && 'm'
            }}</span>
          </span>
        </div>
        <div class="border-r"></div>
        <div class="flex flex-col items-center">
          <span class="">降雨量</span>
          <span class=""
            >{{ currentWeather.WeatherElement.Now.Precipitation }} mm</span
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import overcast from '../assets/icons/overcast.svg'
defineProps({
  currentWeather: {
    type: Object,
    default: () => ({}),
  },
  weatherIcon: {
    type: Object,
    default: () => ({}),
  },
})
// {
//       GeoInfo: { CountyName: '' },

//       StationName: '',
//       WeatherElement: {
//         AirTemperature: 0,
//         Weather: '',
//         DailyExtreme: {
//           DailyHigh: { TemperatureInfo: { AirTemperature: 0 } },
//           DailyLow: { TemperatureInfo: { AirTemperature: 0 } },
//         },
//         UVIndex: 0,
//         WindSpeed: 0,
//         VisibilityDescription: 0,
//         Now: { Precipitation: 0 },
//       },
//     }
</script>

<style lang="scss" scoped></style>
