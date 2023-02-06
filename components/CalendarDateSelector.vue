<template>
    <div class="calendar-date-selector">
      <a @click="selectPrevious"> &#8249; <span> {{ previousMonth }} breakdown</span> </a>
      <a @click="selectNext"> <span>{{ nextMonth }} breakdown</span> &#8250; </a>
    </div>
  </template>
  
  <script setup>
  import dayjs from "dayjs";
  import { defineEmits } from 'vue'

    const emit = defineEmits(['dateSelected' ]);

    const props = defineProps({
      currentDate: {
        type: String,
        required: true
      },
  
      selectedDate: {
        type: Object,
        required: true
      },

    });

    const previousMonth = computed(() => {
      return  dayjs(props.selectedDate).subtract(1, "month").format("MMMM");
    });

    const nextMonth = computed(() => {
      return  dayjs(props.selectedDate).add(1, "month").format("MMMM");
    });

    const selectPrevious = () => {
      let newSelectedDate = dayjs(props.selectedDate).subtract(1, "month");
      emit("dateSelected", newSelectedDate);
    };

    const selectCurrent = () => {
      let newSelectedDate = dayjs(props.currentDate);
      emit("dateSelected", newSelectedDate);
    };

    const selectNext = () => {
      let newSelectedDate = dayjs(props.selectedDate).add(1, "month");
      emit("dateSelected", newSelectedDate);
    };
  </script>
  
  <style scoped lang="scss">
  .calendar-date-selector {
    display: flex;
    justify-content: space-between;
    width: 100%;
    box-sizing: border-box;
    padding: 0;
    font-weight: bold;
    color: #444;
    font-size: 20px;

    span {
      font-size: 11px;
      line-height: 25px;
      display: inline-block;
      transform: translateY(-1px);
    }

    & > * {
      cursor: pointer;
      user-select: none;
    }
  }
  </style>
  