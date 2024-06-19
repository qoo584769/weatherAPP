<script setup>
import { onMounted, ref, computed, watch } from 'vue'
import axios from 'axios'

import CitySelect from './components/CitySelect.vue'
import CurrentWeatherComponent from './components/CurrentWeatherComponent.vue'
import HourlyComponent from './components/HourlyComponent.vue'
import DailyComponent from './components/DailyComponent.vue'

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

const currentWeatherAPI = async (StationId = 466920) => {
  try {
    let url = ''
    const autoStationId = ['C0D660', 'C0K330', 'C0M790']
    if (autoStationId.includes(StationId)) {
      url = `https://opendata.cwa.gov.tw/api/v1/rest/datastore/O-A0001-001?Authorization=${
        import.meta.env.VITE_API_KEY
      }&StationId=${StationId}`
    } else {
      url = `https://opendata.cwa.gov.tw/api/v1/rest/datastore/O-A0003-001?Authorization=${
        import.meta.env.VITE_API_KEY
      }&StationId=${StationId}`
    }
    const response = await axios.get(url)
    const weatherData = response.data.records.Station[0]
    currentWeather.value = weatherData
  } catch (error) {
    console.log('取得及時天氣失敗')
    console.log(error)
  }
}

const hourlyWeatherAPI = async (cityId = 315078) => {
  try {
    const resHourly = await axios.get(
      `https://dataservice.accuweather.com/forecasts/v1/hourly/12hour/${cityId}?apikey=${
        import.meta.env.VITE_ACCUWEATHER_API_KEY
      }&language=zh-tw&metric=true`
    )
    hourlyWeather.value = resHourly.data
  } catch (error) {
    console.log('取得每小時天氣失敗')
    console.log(error)
  }
}

const dailyWeatherAPI = async (cityId = 315078) => {
  try {
    const config = {
      // Headers: {
      //   'x-rapidapi-key': import.meta.env.VITE_RAPIDAPI_API_KEY,
      //   'x-rapidapi-host': 'ai-weather-by-meteosource.p.rapidapi.com',
      // },
    }
    // const url = `https://ai-weather-by-meteosource.p.rapidapi.com/daily?lat=${lat}&lon=${lon}&language=en&units=metric`
    const url = `https://dataservice.accuweather.com/forecasts/v1/daily/5day/${cityId}?apikey=${
      import.meta.env.VITE_ACCUWEATHER_API_KEY
    }&language=zh-tw&metric=true`
    const resDaily = await axios.get(url)
    futureForecasts.value = resDaily.data.DailyForecasts
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

onMounted(async () => {
  try {
    // 現在資料
    await currentWeatherAPI()
    // 每小時資料
    await hourlyWeatherAPI()
    // 每日資料
    await dailyWeatherAPI()
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
  <div class="h-screen w-full bg-no-repeat overflow-auto" :class="bgSelector">
    <div class="flex w-full h-max md:h-screen bg-black/[.1]">
      <div
        class="relative w-full max-w-[1200px] overflow-hidden h-fit m-auto flex flex-col text-center"
      >
        <city-select @call-cur-api="currentWeatherAPI"></city-select>

        <current-weather-component
          :currentWeather="currentWeather"
          :weatherIcon="weatherIcon"
        ></current-weather-component>

        <div class="w-full md:flex justify-between items-end">
          <hourly-component
            :hourlyWeather="hourlyWeather"
            :weatherIcon="weatherIcon"
          ></hourly-component>

          <daily-component
            :futureForecasts="futureForecasts"
            :weatherIcon="weatherIcon"
          ></daily-component>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
