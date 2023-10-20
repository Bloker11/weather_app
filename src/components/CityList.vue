<template>
    <div v-for="city in savedCities" :key="city.id">
        <CityCard :city="city" @click="getToCity(city)"/>
    </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import { useRouter } from "vue-router";
import CityCard from "./CityCard.vue";

const savedCities = ref([]);
const getCities = async () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(
      localStorage.getItem("savedCities")
    );

    const requests = [];
    savedCities.value.forEach((city) => {
      requests.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=80af732f74145cc9e1745fecf2fb8454&units=metric`
        )
      );
    });

    const weatherData = await Promise.all(requests);

    weatherData.forEach((value, index) => {
      savedCities.value[index].weather = value.data;
    });
  }
};
await getCities();

const router = useRouter()

const getToCity = (city) => {
    router.push(
        {
            name:"cityView",
            params: {state:city.state, city:city.city},
            query: {id: city.id, lat: city.coords.lat, lng: city.coords.lng}
    }
    )
}



</script>
