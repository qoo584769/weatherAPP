<script setup>
import axios from 'axios'
import { onMounted, ref, computed } from 'vue'
import precipIcon from './assets/icons/precip-icon.svg'
import mostlyCloudyNight from './assets/icons/mostly-cloudy-night.svg'
import mostlyCloudy from './assets/icons/mostly-cloudy.svg'
import overcast from './assets/icons/overcast.svg'
import partlyCloudyNight from './assets/icons/partly-cloudy-night.svg'
import partlyCloudy from './assets/icons/partly-cloudy.svg'
import mostlyCloudyShower from './assets/icons/mostly-cloudy-shower.svg'
import thunderstorm from './assets/icons/thunderstorm.svg'
import rain from './assets/icons/rain.svg'

const bgWeather = ref({
  雨天: "bg-[url('./assets/image/bg-rain.jpg')]",
  雨: "bg-[url('./assets/image/bg-rain.jpg')]",
  晴天: "bg-[url('./assets/image/bg-sunny.jpg')]",
  晴: "bg-[url('./assets/image/bg-sunny.jpg')]",
  陰: "bg-[url('./assets/image/bg-overcast.jpg')]",
  有霧: "bg-[url('./assets/image/bg-foggy.jpg')]",
  陰有靄: "bg-[url('./assets/image/bg-foggy.jpg')]",
})
const weatherIcon = ref({
  偶雲: partlyCloudy,
  多雲: mostlyCloudy,
  陣雨: mostlyCloudyShower,
  有雷雨: thunderstorm,
  陰: overcast,
})
// 及時天氣
const currentWeather = ref({
  GeoInfo: { CountyName: '' },

  StationName: '',
  WeatherElement: {
    AirTemperature: 0,
    Weather: '',
    DailyExtreme: {
      DailyHigh: { TemperatureInfo: { AirTemperature: 0 } },
      DailyLow: { TemperatureInfo: { AirTemperature: 0 } },
    },
    UVIndex: 0,
    WindSpeed: 0,
    VisibilityDescription: 0,
    Now: { Precipitation: 0 },
  },
})
// 未來五天天氣預報
const futureForecasts = ref([
  {
    Date: '2024-06-17T07:00:00+08:00',
    EpochDate: 1718578800,
    Temperature: {
      Minimum: {
        Value: 26.9,
        Unit: 'C',
        UnitType: 17,
      },
      Maximum: {
        Value: 32.2,
        Unit: 'C',
        UnitType: 17,
      },
    },
    Day: {
      Icon: 7,
      IconPhrase: '多雲',
      HasPrecipitation: true,
      PrecipitationType: 'Rain',
      PrecipitationIntensity: 'Light',
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '02',
      },
    },
    Night: {
      Icon: 12,
      IconPhrase: '陣雨',
      HasPrecipitation: true,
      PrecipitationType: 'Rain',
      PrecipitationIntensity: 'Light',
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '03',
      },
    },
    Sources: ['AccuWeather', '華風氣象傳媒'],
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=1&unit=c&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=1&unit=c&lang=zh-tw',
  },
  {
    Date: '2024-06-18T07:00:00+08:00',
    EpochDate: 1718665200,
    Temperature: {
      Minimum: {
        Value: 25.1,
        Unit: 'C',
        UnitType: 17,
      },
      Maximum: {
        Value: 30.6,
        Unit: 'C',
        UnitType: 17,
      },
    },
    Day: {
      Icon: 15,
      IconPhrase: '有雷雨',
      HasPrecipitation: true,
      PrecipitationType: 'Rain',
      PrecipitationIntensity: 'Moderate',
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '04',
      },
    },
    Night: {
      Icon: 38,
      IconPhrase: '多雲',
      HasPrecipitation: false,
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '02',
      },
    },
    Sources: ['AccuWeather', '華風氣象傳媒'],
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=2&unit=c&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=2&unit=c&lang=zh-tw',
  },
  {
    Date: '2024-06-19T07:00:00+08:00',
    EpochDate: 1718751600,
    Temperature: {
      Minimum: {
        Value: 25.4,
        Unit: 'C',
        UnitType: 17,
      },
      Maximum: {
        Value: 33.3,
        Unit: 'C',
        UnitType: 17,
      },
    },
    Day: {
      Icon: 15,
      IconPhrase: '有雷雨',
      HasPrecipitation: true,
      PrecipitationType: 'Rain',
      PrecipitationIntensity: 'Moderate',
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '04',
      },
    },
    Night: {
      Icon: 35,
      IconPhrase: '多雲時陰',
      HasPrecipitation: false,
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '01',
      },
    },
    Sources: ['AccuWeather', '華風氣象傳媒'],
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=3&unit=c&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=3&unit=c&lang=zh-tw',
  },
  {
    Date: '2024-06-20T07:00:00+08:00',
    EpochDate: 1718838000,
    Temperature: {
      Minimum: {
        Value: 26.3,
        Unit: 'C',
        UnitType: 17,
      },
      Maximum: {
        Value: 34.1,
        Unit: 'C',
        UnitType: 17,
      },
    },
    Day: {
      Icon: 6,
      IconPhrase: '多雲',
      HasPrecipitation: true,
      PrecipitationType: 'Rain',
      PrecipitationIntensity: 'Light',
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '01',
      },
    },
    Night: {
      Icon: 35,
      IconPhrase: '多雲時陰',
      HasPrecipitation: false,
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '01',
      },
    },
    Sources: ['AccuWeather', '華風氣象傳媒'],
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=4&unit=c&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=4&unit=c&lang=zh-tw',
  },
  {
    Date: '2024-06-21T07:00:00+08:00',
    EpochDate: 1718924400,
    Temperature: {
      Minimum: {
        Value: 26.6,
        Unit: 'C',
        UnitType: 17,
      },
      Maximum: {
        Value: 34.9,
        Unit: 'C',
        UnitType: 17,
      },
    },
    Day: {
      Icon: 6,
      IconPhrase: '多雲',
      HasPrecipitation: false,
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '01',
      },
    },
    Night: {
      Icon: 38,
      IconPhrase: '多雲',
      HasPrecipitation: false,
      LocalSource: {
        Id: 7,
        Name: 'Huafeng',
        WeatherCode: '02',
      },
    },
    Sources: ['AccuWeather', '華風氣象傳媒'],
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=5&unit=c&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/daily-weather-forecast/315078?day=5&unit=c&lang=zh-tw',
  },
])
// 每小時天氣
const hourlyWeather = ref([
  {
    DateTime: '2024-06-16T23:00:00+08:00',
    EpochDateTime: 1718550000,
    WeatherIcon: 36,
    IconPhrase: '偶雲',
    HasPrecipitation: false,
    IsDaylight: false,
    Temperature: {
      Value: 30.1,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 40,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=1&hbhhour=23&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=1&hbhhour=23&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T00:00:00+08:00',
    EpochDateTime: 1718553600,
    WeatherIcon: 12,
    IconPhrase: '陣雨',
    HasPrecipitation: true,
    PrecipitationType: 'Rain',
    PrecipitationIntensity: 'Light',
    IsDaylight: false,
    Temperature: {
      Value: 28.2,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 44,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=0&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=0&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T01:00:00+08:00',
    EpochDateTime: 1718557200,
    WeatherIcon: 7,
    IconPhrase: '多雲',
    HasPrecipitation: false,
    IsDaylight: false,
    Temperature: {
      Value: 30.5,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 44,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=1&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=1&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T02:00:00+08:00',
    EpochDateTime: 1718560800,
    WeatherIcon: 7,
    IconPhrase: '多雲',
    HasPrecipitation: false,
    IsDaylight: false,
    Temperature: {
      Value: 34.6,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 47,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=2&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=2&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T03:00:00+08:00',
    EpochDateTime: 1718564400,
    WeatherIcon: 12,
    IconPhrase: '陣雨',
    HasPrecipitation: true,
    PrecipitationType: 'Rain',
    PrecipitationIntensity: 'Light',
    IsDaylight: false,
    Temperature: {
      Value: 29.4,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 51,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=3&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=3&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T04:00:00+08:00',
    EpochDateTime: 1718568000,
    WeatherIcon: 7,
    IconPhrase: '多雲',
    HasPrecipitation: false,
    IsDaylight: false,
    Temperature: {
      Value: 32.8,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 47,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=4&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=4&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T05:00:00+08:00',
    EpochDateTime: 1718571600,
    WeatherIcon: 7,
    IconPhrase: '多雲',
    HasPrecipitation: false,
    IsDaylight: false,
    Temperature: {
      Value: 33.6,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 47,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=5&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=5&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T06:00:00+08:00',
    EpochDateTime: 1718575200,
    WeatherIcon: 12,
    IconPhrase: '陣雨',
    HasPrecipitation: true,
    PrecipitationType: 'Rain',
    PrecipitationIntensity: 'Light',
    IsDaylight: true,
    Temperature: {
      Value: 30.9,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 51,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=6&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=6&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T07:00:00+08:00',
    EpochDateTime: 1718578800,
    WeatherIcon: 7,
    IconPhrase: '多雲',
    HasPrecipitation: false,
    IsDaylight: true,
    Temperature: {
      Value: 35.2,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 25,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=7&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=7&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T08:00:00+08:00',
    EpochDateTime: 1718582400,
    WeatherIcon: 7,
    IconPhrase: '多雲',
    HasPrecipitation: false,
    IsDaylight: true,
    Temperature: {
      Value: 34.9,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 20,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=8&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=8&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T09:00:00+08:00',
    EpochDateTime: 1718586000,
    WeatherIcon: 7,
    IconPhrase: '多雲',
    HasPrecipitation: false,
    IsDaylight: true,
    Temperature: {
      Value: 31.5,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 20,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=9&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=9&lang=zh-tw',
  },
  {
    DateTime: '2024-06-17T10:00:00+08:00',
    EpochDateTime: 1718589600,
    WeatherIcon: 7,
    IconPhrase: '多雲',
    HasPrecipitation: false,
    IsDaylight: true,
    Temperature: {
      Value: 33.6,
      Unit: 'F',
      UnitType: 18,
    },
    PrecipitationProbability: 20,
    MobileLink:
      'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=10&lang=zh-tw',
    Link: 'http://www.accuweather.com/zh/tw/taipei-city/315078/hourly-weather-forecast/315078?day=2&hbhhour=10&lang=zh-tw',
  },
])

const currentIndex = ref(0)
const numForecasts = hourlyWeather.value.length

const slide = (direction) => {
  if (direction === 'left') {
    currentIndex.value = (currentIndex.value - 1 + numForecasts) % numForecasts
  } else if (direction === 'right') {
    currentIndex.value = (currentIndex.value + 1) % numForecasts
  }
}

const currentWeatherAPI = async (StationName = '臺北') => {
  try {
    const response = await axios.get(
      `https://opendata.cwa.gov.tw/api/v1/rest/datastore/O-A0003-001?Authorization=${
        import.meta.env.VITE_API_KEY
      }&StationName=${StationName}`
    )
    const weatherData = response.data.records.Station[0]
    return weatherData
  } catch (error) {
    console.log('取得及時天氣失敗')
    console.log(error)
  }
}

const hourlyWeatherAPI = async () => {
  try {
    const resHourly = await axios.get(
      `https://dataservice.accuweather.com/forecasts/v1/hourly/12hour/315078?apikey=${
        import.meta.env.VITE_ACCUWEATHER_API_KEY
      }&language=zh-tw&metric=true`
    )
    return resHourly.data
  } catch (error) {
    console.log('取得每小時天氣失敗')
    console.log(error)
  }
}

const dailyWeatherAPI = async (lat = 25.033493, lon = 121.564101) => {
  try {
    const config = {
      // Headers: {
      //   'x-rapidapi-key': import.meta.env.VITE_RAPIDAPI_API_KEY,
      //   'x-rapidapi-host': 'ai-weather-by-meteosource.p.rapidapi.com',
      // },
    }
    // const url = `https://ai-weather-by-meteosource.p.rapidapi.com/daily?lat=${lat}&lon=${lon}&language=en&units=metric`
    const url = `https://dataservice.accuweather.com/forecasts/v1/daily/5day/315078?apikey=${
      import.meta.env.VITE_ACCUWEATHER_API_KEY
    }&language=zh-tw&metric=true`
    const resDaily = await axios.get(url)
    return resDaily.data.DailyForecasts
    // return resDaily.data.daily.data
  } catch (error) {
    console.log('取得每日天氣失敗')
    console.log(error)
  }
}

const bgSelector = computed(() => {
  return (
    bgWeather.value[currentWeather.value?.WeatherElement?.Weather] ||
    "bg-[url('./assets/image/bg-overcast.jpg')]"
  )
})

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

const featureDayWeather = computed(() => {
  let data = []
  futureForecasts.value.forEach((item, index) => {
    item.Day.Icon = weatherIcon.value[item.Day.IconPhrase]
    data.push(item)
  })
  return data
})

onMounted(async () => {
  try {
    // 現在資料
    currentWeather.value = await currentWeatherAPI()
    // 每小時資料
    hourlyWeather.value = await hourlyWeatherAPI()
    // 每日資料
    futureForecasts.value = await dailyWeatherAPI()
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
  <div class="h-screen w-full bg-no-repeat overflow-auto" :class="bgSelector">
    <div class="flex w-full h-max md:h-screen bg-black/[.1]">
      <div class="w-full max-w-[1200px] h-fit m-auto flex flex-col text-center">
        <div
          class="m-4 bg-black/[.05] shadow-[0_0_10px_rgba(255,255,255,0.1)] border-2 border-white/[.1] backdrop-blur text-white py-5 md:p-5 md:m-0 md:mb-10 rounded-3xl"
        >
          <div class="text-5xl mb-4">
            {{ currentWeather?.WeatherElement?.AirTemperature }} °C
          </div>
          <div
            class="text-3xl font-semibold mb-4 flex items-center justify-center"
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
          <div class="flex fle-col items-center justify-center mb-3"></div>
          <div class="hidden">
            <span class=""
              >H:{{
                currentWeather.WeatherElement.DailyExtreme.DailyHigh
                  ?.TemperatureInfo?.AirTemperature
              }}
              °C</span
            >
            <span class=""
              >L:{{
                currentWeather.WeatherElement.DailyExtreme.DailyLow
                  ?.TemperatureInfo?.AirTemperature
              }}
              °C</span
            >
          </div>
          <div
            class="flex md:gap-4 flex-wrap justify-evenly md:justify-around text-base md:text-lg"
          >
            <div class="flex flex-col items-center">
              <span class="">紫外線</span>
              <span class=""
                >{{ currentWeather.WeatherElement.UVIndex }} UVI</span
              >
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
                  currentWeather.WeatherElement.VisibilityDescription
                }}
                m</span
              >
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

        <div class="w-full md:flex justify-between items-end">
          <div
            class="mx-4 h-fit bg-black/[.05] shadow-[0_0_10px_rgba(255,255,255,0.1)] border-2 border-white/[.1] backdrop-blur p-3 md:m-0 md:p-5 rounded-3xl"
          >
            <h2 class="text-2xl font-bold mb-4 text-white">每小時預報</h2>
            <div
              class="flex justify-center items-center overflow-hidden relative"
            >
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
                        :alt="dailyForecast.description"
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
      </div>
    </div>
  </div>
</template>

<style scoped></style>