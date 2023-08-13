<template>
    <div class="body-overview flex flex-col items-center text-white py-12">
      <h1 class="text-4xl mb-2">{{ cityName }}</h1>
      <p class="text-sm mb-12">
        {{
          new Date(weatherData.currentTime).toLocaleDateString(
            "en-uk",
            {
              weekday: "short",
              day: "2-digit",
              month: "long",
            }
          )
        }}
        {{
          new Date(weatherData.currentTime).toLocaleTimeString(
            "en-uk",
            {
              timeStyle: "short",
            }
          )
        }}
      </p>
      <p class="text-8xl mb-8">
        {{ Math.round(weatherData.current.temp) }}&deg;
      </p>
      <p>
        Feels like
        {{ Math.round(weatherData.current.feels_like) }} &deg;
      </p>
      <p class="capitalize">
        {{ weatherData.current.weather[0].description }}
      </p>
      <img
        class="w-[150px] h-auto"
        :src="`http://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`"
        alt=""
      />
    </div>
  </template>
  
  <script>
  export default {
    props: {
      cityName: {
        type: String,
        required: true,
      },
      weatherData: {
        type: Object,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  /* Add your styles here */
  </style>