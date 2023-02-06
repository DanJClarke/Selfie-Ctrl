<template>
  <div class="calendar-month">
    <div class="calendar-month-header">
      <CalendarDateIndicator
        :selected-date="selectedDate"
        class="calendar-month-header-selected-month"
      />
    </div>
    
    <CalendarWeekdays/>

    <ul class="days-grid">
      <CalendarMonthDayItem
        v-for="day in days"
        :key="day.date"
        :day="day"
        :is-today="day.date === today"
      />
    </ul>
    <CalendarDateSelector
        :current-date="today"
        :selected-date="selectedDate"
        :previous-month="previousMonth"
        :next-month="nextMonth"
        @dateSelected="selectDate"
      />
  </div>
</template>

<script setup>
import { ref, computed } from "@vue/composition-api";
import dayjs from "dayjs";
import weekday from "dayjs/plugin/weekday";
import weekOfYear from "dayjs/plugin/weekOfYear";

  
    dayjs.extend(weekday);
    dayjs.extend(weekOfYear);
    const selectedDate = ref(dayjs());

    const days = computed(() => {
      return [
        ...previousMonthDays.value,
        ...currentMonthDays.value,
        ...nextMonthDays.value,
      ];
    });

    const today = computed(() => {
      return dayjs().format("YYYY-MM-DD");
    });
    const month = computed(() => {
      return Number(selectedDate.value.format("M"));
    });

    const year = computed(() => {
      return Number(selectedDate.value.format("YYYY"));
    });

    const numberOfDaysInMonth = computed(() => {
      return dayjs(selectedDate.value).daysInMonth();
    });

    const currentMonthDays = computed(() => {
      return [...Array(numberOfDaysInMonth.value)].map((day, index) => {
        return {
          date: dayjs(`${year.value}-${month.value}-${index + 1}`).format(
            "YYYY-MM-DD"
          ),
          isCurrentMonth: true,
        };
      });
    });

    const previousMonth = computed(() => {
      return monthsTimeLine('subtract');
    });

    const nextMonth = computed(() => {
      return monthsTimeLine('add');
    });

    const previousMonthDays = computed(() => {
      const firstDayOfTheMonthWeekday = getWeekday(currentMonthDays.value[0].date);
      const previousMonth = monthsTimeLine('subtract');
      const visibleNumberOfDaysFromPreviousMonth = firstDayOfTheMonthWeekday
        ? firstDayOfTheMonthWeekday - 1
        : 6;
      const previousMonthLastMondayDayOfMonth = dayjs(
        currentMonthDays.value[0].date
      )
        .subtract(visibleNumberOfDaysFromPreviousMonth, "day")
        .date();
      return [...Array(visibleNumberOfDaysFromPreviousMonth)].map(
        (day, index) => {
          return {
            date: dayjs(
              `${previousMonth.year()}-${previousMonth.month() + 1}-${
                previousMonthLastMondayDayOfMonth + index
              }`
            ).format("YYYY-MM-DD"),
            isCurrentMonth: false,
          };
        }
      );
    });

    const nextMonthDays = computed(() => {
      const lastDayOfTheMonthWeekday = getWeekday(
        `${year.value}-${month.value}-${currentMonthDays.value.length}`
      );
      const nextMonth = monthsTimeLine('add');
      const visibleNumberOfDaysFromNextMonth = lastDayOfTheMonthWeekday
        ? 7 - lastDayOfTheMonthWeekday
        : lastDayOfTheMonthWeekday;
      return [...Array(visibleNumberOfDaysFromNextMonth)].map((day, index) => {
        return {
          date: dayjs(
            `${nextMonth.year()}-${nextMonth.month() + 1}-${index + 1}`
          ).format("YYYY-MM-DD"),
          isCurrentMonth: false,
        };
      });
    });

    const monthsTimeLine = (direction) => {
      return dayjs(`${year.value}-${month.value}-01`)[`${ direction }`](1,  "month");
    }

    const getWeekday = (date) => {
      return dayjs(date).weekday();
    };

    const selectDate = (newSelectedDate) => {
      selectedDate.value = newSelectedDate;
    };
</script>

<style scoped lang="scss">
.calendar-month {
  position: relative;
}

.weekdays {
  font-size: 18px;
  background-color: #fff;

  & > * {
    text-align: right;
    padding-right: 5px;
  }
}


.weekdays,
.days-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.days-grid {
  height: 100%;
  padding: 0px 0 20px 0;
  position: relative;
  grid-column-gap: 5px;
  grid-row-gap: 5px;
}
</style>