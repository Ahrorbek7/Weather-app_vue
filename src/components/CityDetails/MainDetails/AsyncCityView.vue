<template>
  <div class="flex flex-col flex-1 items-center">
    <div
      v-if="route.query.preview"
      class="text-white p-4 bg-weather-secondary w-full text-center"
    >
      <p>
        You are currently previewing this city, click the "+"
        icon to start tracking this city.
      </p>
    </div>
    <!-- Body Overview -->
     <CityBodyOverview :city-name="route.params.city" :weather-data="weatherData" />

    <hr class="border-white border-opacity-10 border w-full" />

    <!-- Hourly Weather -->
      <HourlyWeather :weather-data="weatherData"/>

    <hr class="border-white border-opacity-10 border w-full" />

    <!-- Weekly Weather -->
      <WeeklyWeather  :weather-data="weatherData"/>
      <RemoveBtn />
  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute, useRouter } from "vue-router";
import WeeklyWeather from "../CityWeather/WeeklyWeather.vue";
import HourlyWeather from "../CityWeather/HourlyWeather.vue";
import CityBodyOverview from "../CityWeather/CityBodyOverview.vue";
import RemoveBtn from "../../Equipment/RemoveBtn.vue";

const route = useRoute();
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=imperial`
    );
    // cal current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime =
      utc + 1000 * weatherData.data.timezone_offset;

    // cal hourly weather offset
    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime =
        utc + 1000 * weatherData.data.timezone_offset;
    });

    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};
const weatherData = await getWeatherData();

const router = useRouter();
const removeCity = () => {
  const cities = JSON.parse(localStorage.getItem("savedCities"));
  const updatedCities = cities.filter(
    (city) => city.id !== route.query.id
  );
  localStorage.setItem(
    "savedCities",
    JSON.stringify(updatedCities)
  );
  router.push({
    name: "home",
  });
};
</script>
