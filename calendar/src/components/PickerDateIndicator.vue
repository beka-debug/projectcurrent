<template>
  <div class="selected-month">
    <span @click="selectPrevious" class="arrow"><</span>
    <div
    v-on:click="showMonth"
    class="calendar-date-indicator">{{ selectedMonth }} <span class="rotatearrow">></span></div>
    <span @click="selectNext" class="arrow">></span>
  </div>
  </template>
  
  <script>
  import dayjs from "dayjs";
  
  export default {
    data(){
       return{
        Months: ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov","Dec"],
        show:false
       }
    },
    props: {
      selectedDate: {
        type: Object,
        required: true
      }
    },
  
    computed: {
      selectedMonth() {
        return this.Months[this.selectedDate.month()];
      }
    },
    methods: {
      showMonth(){
        //this.show = !this.show
        this.$emit("showmonth",true)
        console.log("adasdas")
       // console.log(this.show)

      },
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
    font-size: 12px;
    font-weight: 600;
    color: #3C3C3B;
    
    align-items: center;
    justify-content: space-between;
  }
  .rotatearrow{
    transform: rotate(90deg) !important;
    display: inline-block;


    font-size: 10px;
    font-weight: normal;
    
  }
  .selected-month{
    display: flex;
    align-items: center;
  }
  .calendar-date-indicator:hover{
    background: #3C3C3B0D 0% 0% no-repeat padding-box;

  }
  .arrow{
    color: #3C3C3B4D;
    font-weight: bold;
    margin: 0px 9px;
  }
  </style>
  
  