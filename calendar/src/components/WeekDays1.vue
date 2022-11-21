<template>
    <ol class="day-of-week">
      <li v-for="weekday in weekdays" :key="weekday">{{ weekday }}</li>
    </ol>
  </template>
  
  <script>
  const WEEKDAYS = ["Mo", "Tu", "We", "Th", "Fr", "Sa", "Su"];
  
  export default {
    name: "WeekDays1",
  
    computed: {
      weekdays() {
        return WEEKDAYS;
      }
    }
  };
  </script>
  
  <style scoped>
  .day-of-week {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
  }
  
  .day-of-week > * {
    text-align: center;
    padding-right: 5px;
  }
  </style>
  