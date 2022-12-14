<template>
    <ol class="day-of-week">
      <li v-for="weekday in weekdays" :key="weekday">{{ weekday }}</li>
    </ol>
  </template>
  
  <script>
  const WEEKDAYS = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];
  
  export default {
    name: "PickerWeekday",
  
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
    font-size: 14px;
    color: #3C3C3B;
    font: normal normal normal 14px/27px Poppins;
letter-spacing: 0.28px;
color: #3C3C3B;
opacity: 0.9;

  }
  
  .day-of-week > * {
    text-align: right;
    padding-right: 5px;
  }
  </style>
  