<template>
    <div class="month-selector">
      <span @click="selectPrevious"><</span>
      <div 
      v-on:click="showMonth"
      class="calendar-date-indicator">{{ selectedMonth }}</div>
      <span @click="selectNext">></span>
    </div>
    </template>
    
    <script>
    import dayjs from "dayjs";
    export default {
  
      props: {
        selectedDatee: {
          type: Object,
          required: true
        }
      },
    
      computed: {
        selectedMonth() {
          return this.selectedDatee.format("MMMM");
        }
      },
      methods: {
        showMonth(){
           this.show = !this.show
           this.$emit("showmonth",this.show)
           console.log(this.show)
      },
        selectPrevious() {
        let newSelectedDate = dayjs(this.selectedDatee).subtract(1, "month");
        this.$emit("dateSelected", newSelectedDate);
      },
      selectNext() {
        let newSelectedDate = dayjs(this.selectedDatee).add(1, "month");
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
    .month-selector{
      display: flex;
      align-items: center;
      justify-content: space-around;
    }
    </style>
    
    