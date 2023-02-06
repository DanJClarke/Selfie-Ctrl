<template>
    <li
      class="calendar-day"
      :class="{ 'calendar-day--not-current': !props.day.isCurrentMonth, 'calendar-day--today': props.isToday }"
    >
      <span>{{ label }}</span>
    </li>
  </template>
  
<script setup>
  import { computed } from 'vue';
  import dayjs from "dayjs";

  const props = defineProps({
    day: {
      type: Object,
      required: true
    },
    isCurrentMonth: {
      type: Boolean,
      default: false
    },
    isToday: {
      type: Boolean,
      default: false
    }
  });

  const label = computed(() => {
    return dayjs(props.day.date).format("D");
  });

</script>
  
<style scoped lang="scss">
.calendar-day {
  position: relative;
  font-size: 16px;
  background-color: #D9D9D9;
  color: #fff;
  border-radius: 5px;
  padding: 5px;
  padding-bottom: 100%!important;

  span {
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    right: 2px;
    font-weight: bold;
    font-size: 14px;
  }
  &--not-current {
    background-color: #fff;
    
    span {
      display: none;
    }
  }
}
</style>
  