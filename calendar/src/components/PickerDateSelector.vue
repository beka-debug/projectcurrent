<template>
    <div class="calendar-date-selector">
      <span @click="selectPrevious" class="arrow"><</span>
      <span @click="selectCurrent">{{selectedDate.format("YYYY")}}</span>
      <span @click="selectNext" class="arrow">></span>
    </div>
  </template>
  
  <script>
  import dayjs from "dayjs";
  
  export default {
    name: "PickerModeSelector",
  
    props: {
      currentDate: {
        type: String,
        required: true
      },
  
      selectedDate: {
        type: Object,
        required: true
      }
    },
  
    methods: {

    
      selectPrevious() {
        let newSelectedDate = dayjs(this.selectedDate).subtract(1, "year");
        this.$emit("dateSelected", newSelectedDate);
      },
  
      selectCurrent() {
        let newSelectedDate = dayjs(this.currentDate);
        this.$emit("dateSelected", newSelectedDate);
        this.$emit("showYearFuncEmitter",true)
        
      },
  
      selectNext() {
        let newSelectedDate = dayjs(this.selectedDate).add(1, "year");
        this.$emit("dateSelected", newSelectedDate);
      }
    }
  };
  </script>
  
  <style scoped>
  .calendar-date-selector {
    display: flex;
    justify-content: space-between;
    font-size: 12px;
    color:#3C3C3B;
  }
  
  .calendar-date-selector > * {
    cursor: pointer;
    user-select: none;
  }
  .arrow{
    color: #3C3C3B4D;
    font-weight: bold;
    margin: 0px 9px;
  }
  </style>
  