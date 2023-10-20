<template>
    <div class="flex flex-col flex-1 items-center">
        <div v-if="route.query.preview" class="bg-weather-secondary w-full text-white p-4 text-center">
            If you want to add this city/region to your list please click "+" button.
        </div>
        <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
            <p class="text-sm mb-12">
                {{ 
                    new Date(weatherData.list[0].dt).toLocaleDateString(
                        "en-us",
                        {
                            weekday: "short",
                            day: "2-digit",
                            month: "long"
                        }
                    )
                }}
                {{ 
                    new Date(weatherData.list[0].dt).toLocaleTimeString(
                        "it-IT",
                        {
                            timeStyle: "short"
                        }
                    )
                }}
            </p>
            <p class="text-8xl mb-8">
                {{ Math.round(weatherData.list[0].main.temp) }}&deg;C
            </p>
            <div class="text-center">
                <p>Feels like: {{ Math.round(weatherData.list[0].main.feels_like) }}&deg;C</p>
                <p class="capitalize">Conditions: {{ weatherData.list[0].weather[0].description }}</p>
                <img class="w-[150px] h-auto" :src="`https://openweathermap.org/img/wn/${weatherData.list[0].weather[0].icon}@2x.png`" alt="">
            </div>
            
        </div>
        <hr class="border-white border-opacity-10 border w-full"/>
        <div class=" w-full py-12">
            <div class="mx-8 text-white">
                <h2 class="mb-4">5 day 3 hour forecast</h2>
                <div class="flex gap-10 overflow-x-scroll">
                    <div v-for="hourData in weatherData.list" :key="hourData.dt" class="flex flex-col gap-4 items-center">
                        <p class="whitespace-nowrap text-md">
                            <!-- {{ 
                                new Date(hourData.dt).toLocaleTimeString("en-us", {
                                    hour: "numeric"
                                })    
                            }} -->
                            {{ hourData.dt_txt }}
                        </p>
                        <img class="w-auto h-[50px] object-cover" :src="`https://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`" alt="">
                        <p class="text-xl">
                            {{ Math.round(hourData.main.temp) }}&deg;C
                        </p>
                    </div>
                </div>
            </div>
            
        </div>
        <div class="flex items-center duration-150 hover:text-red-500 gap-2 py-12 text-white cursor-pointer" @click="removeCity()">
                <i class="fa-solid fa-trash"></i>
                <p>Remove City</p>
            </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();
const getWeatherData = async () => {
    try {
        const weatherData = await axios.get(`https://api.openweathermap.org/data/2.5/forecast?lat=${route.query.lat}&lon=${route.query.lng}&appid=80af732f74145cc9e1745fecf2fb8454&units=metric`)

        

        console.log(weatherData);
        return weatherData.data

    } catch (error) {
        console.log(error);
    }
}
const weatherData = await getWeatherData();
console.log(weatherData);

const router = useRouter()

const removeCity = () => {
    const cities = JSON.parse(localStorage.getItem("savedCities"))
    const updatedCities = cities.filter((city) => city.id !== route.query.id)
    localStorage.setItem("savedCities", JSON.stringify(updatedCities))
    router.push({
        name: "home"
    })
}
</script>

