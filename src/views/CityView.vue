<template>
  <div>
    <Suspense>
      <template #default>
        <AsyncCityView />
      </template>
      <template #fallback>
        <CityViewSkeleton />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCityView from "../components/CityDetails/MainDetails/AsyncCityView.vue";
import CityViewSkeleton from "../components/CityDetails/Loaders/CitySkeleton.vue";
</script>
