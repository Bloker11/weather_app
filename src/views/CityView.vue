<template>
    <div>
        <Suspense>
            <AsyncCityView />
            <template #fallback>
                <CityViewSkeleton/>
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import CityViewSkeleton from '../components/CityViewSkeleton.vue';
import AsyncCityView from './AsyncCity.vue';
</script>
