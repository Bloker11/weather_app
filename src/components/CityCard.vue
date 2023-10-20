<template>
    <div>
        <div class="flex bg-weather-secondary px-6 py-3 rounded-md shadow-md cursor-pointer">
            <div class="flex flex-col flex-1">
                <h2 class="text-3xl">{{ city.city }}</h2>
                <h3>{{ city.state }}</h3>
            </div>
            <div class="flex flex-col gap-2">
                <p class="text-3xl self-end">
                    {{ Math.round(city.weather.main.temp) }}&deg;C
                </p>
                <div class="flex gap-2">
                    <p class="text-xs">
                    H: {{ Math.round(city.weather.main.temp_max) }}&deg;C
                    </p>
                    <p class="text-xs">
                    L: {{ Math.round(city.weather.main.temp_min) }}&deg;C
                    </p>
                </div>
                
            </div>
        </div>
        {{ console.log(city) }}
    </div>
</template>

<script setup>
    defineProps({
        city: {
            type: Object,
            default: () => {}
        }
    })
    
</script>
