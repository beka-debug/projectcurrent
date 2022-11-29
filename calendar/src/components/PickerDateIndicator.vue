<template>
  <div class="selected-month">
    <span @click="selectPrevious"><</span>
    <div class="calendar-date-indicator">{{ selectedMonth }}</div>
    <span @click="selectNext">></span>
  </div>
  </template>
  
  <script>
  import dayjs from "dayjs";
  export default {
    props: {
      selectedDate: {
        type: Object,
        required: true
      }
    },
  
    computed: {
      selectedMonth() {
        return this.selectedDate.format("MMMM");
      }
    },
    methods: {
      selectPrevious() {
        let newSelectedDate = dayjs(this.selectedDate).subtract(1, "month");
        this.$emit("dateSelected", newSelectedDate);
      },
  
      selectCurrent() {
        let newSelectedDate = dayjs(this.currentDate);
        this.$emit("dateSelected", newSelectedDate);
      },
  
      selectNext() {
        let newSelectedDate = dayjs(this.selectedDate).add(1, "month");
        this.$emit("dateSelected", newSelectedDate);
      }
    
    },
  };
  </script>
  
  <style scoped>
  .calendar-date-indicator {
    font-size: 24px;
    font-weight: 600;
    color: var(--grey-00);
  }
  .selected-month{
    display: flex;
  }
  </style>
  
  