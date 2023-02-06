<template>
  <div class="calendar-date-indicator">{{ selectedMonth }}'s Selfie breakdown</div>
  <div class="calendar-key">
    <span class="key-unfiltered">Unfiltered selfie images</span>
    <span class="key-filtered">Filtered selfie images</span>
  </div>
</template>

<script setup>
  import { computed } from 'vue';

  const props = defineProps({
    selectedDate: {
      type: Object,
      required: true
    }
  });

  const selectedMonth = computed(() => {
    return props.selectedDate.format("MMMM");
  });
</script>

<style scoped lang="scss">
.calendar-date-indicator {
  font-size: 20px;
  font-weight: 600;
  text-align: left;
}
.calendar-key {
  display: flex;
  padding: 10px 0 0 0;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;

  & > span:before {
    content: "";
    width: 16px;
    height: 16px;
    background: red;
    display: inline-block;
    transform: translateY(3px);
    margin-right: 8px;
    border-radius: 5px;
  }

  .key-unfiltered:before {
    background: #FE0B67;
  }
  .key-filtered:before {
    background: #830071;
  }
}
</style>