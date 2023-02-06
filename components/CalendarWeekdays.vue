<template>
  <ul class="weekdays">
    <li 
      v-for="weekday in weekdays" 
      :key="weekday"
    >
      {{ weekday }}
    </li>
  </ul>
</template>

<script setup>
  import { computed } from 'vue';
  const weekdays = computed(() => {
    return ["M", "Tu", "W", "Th", "F", "Sa", "Su"];
  });
</script>

<style lang="scss" scoped>
.weekdays  {
  padding: 0 ;
  display: grid;
  grid-template-columns: repeat(7, 1fr);

  & > * {
    text-align: center;
    color: #444;
    font-size: 12px;
    padding-right: 5px;
    font-weight: bold;
  }
}
</style>
