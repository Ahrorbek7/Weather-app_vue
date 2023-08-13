<template>
    <div class="max-w-screen-md w-full py-12">
        <div class="mx-8 text-white">
            <h2 class="mb-4">Hourly Weather</h2>
            <div class="flex gap-10 overflow-x-scroll">
                <!-- Add iteration over each hourly weather data -->
                <div v-for="hourData in weatherData.hourly" :key="hourData.dt" class="flex flex-col gap-4 items-center">
                    <p class="whitespace-nowrap text-md">
                        {{
                            new Date(
                                hourData.currentTime
                            ).toLocaleTimeString("en-uk", {
                                hour: "numeric",
                            })
                        }}
                    </p>
                    <img
                      class="w-auto h-[50px] object-cover"
                      :src="
                        `http://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`
                      "
                      alt=""
                    />
                    <p class="text-xl">
                      {{ Math.round(hourData.temp) }}&deg;
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script setup>
import { defineProps } from "vue";

const props = defineProps({
    weatherData: {
        type: Object,
        required: true,
    },
});
</script>