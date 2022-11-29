<template>
    <li
    @click="choosenDate"
      class="calendar-day"
      :class="{
        'calendar-day--not-current': !day.isCurrentMonth,
        'calendar-day--today': isToday,
        'is-selected-date' : day.isSeletedDate
      }"
    >
      <span>{{ label }}</span>
    </li>
  </template>
  
  <script>
  import dayjs from "dayjs";
  
  export default {
    name: "PickerMonthDayItem",
  
    props: {
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
    },
  
    computed: {
      label() {
        return dayjs(this.day.date).format("D");
      }
    },
    methods: {
      choosenDate(){
        this.$emit("choosedateemitter",this.day.date)
        console.log(this.day.date)
      }
    },
  };
  </script>
  
  <style scoped>
  .calendar-day {
    position: relative;
    height: 40px;

    font-size: 16px;
    background-color: #fff;
    color: var(--grey-800);
    display: flex;
    justify-content: center !important;
    align-items: center !important;
  }
  
  .calendar-day > span {
    display: flex;
    justify-content: center;
    align-items: center;
    
    right: 2px;
    width: var(--day-label-size);
    height: var(--day-label-size);
  }
  
  .calendar-day--not-current {
    background-color: var(--grey-100);
    color: var(--grey-300);
  }
  
  .calendar-day--today {
    
  }
  
  .calendar-day--today > span {
    color: #fff;
    border-radius: 9999px;
    background-color: var(--grey-800);
  }
  .is-selected-date{
    background-color: green !important;
  }
  </style>
  