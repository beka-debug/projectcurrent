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
    width: 24px;
    height: 24px;
   
    font-size: 16px;
    

    display: flex;
    justify-content: center !important;
    align-items: center !important;
  }
  
  
  .calendar-day > span {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 14px;
    

  }
  
  .calendar-day--not-current {
    font: normal normal normal 14px/27px Poppins;
letter-spacing: 0.28px;
color: #3C3C3B;
opacity: 0.9;
  }
  
  .calendar-day--today {
    
  }
  

  .is-selected-date{
    background-color: #418200 !important;
    border-radius: 6px;
  }
  li{
    margin:0px 3px;
  }
  </style>
  