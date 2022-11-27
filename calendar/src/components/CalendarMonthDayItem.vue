<template>
  <li 
 
  @click="start_end_date"
  tabindex="1"
    class="calendar-day"
    :class="{
      'calendar-day--not-current': !day.isCurrentMonth,
      'interval-now':day.isInterval,
      'transparent':day.clicked,
      'int':day.first ,
      'int2':day.last,
      
      'weekend':disabledday,
      'fulldate':day.fulldate
       
      // 'calendar-day--today': isToday
    }"
  >
    <span>{{ label  }} </span>
    
  </li>
</template>

<script>
import dayjs from "dayjs";

export default {
  name: "CalendarMonthDayItem",
  data(){
    return{
      arr: [],
      counter:0,
      intervalNow:false,
      show:false
      
    }
  },
  props: {
    day: {
      type: Object,
      required: true
    },

    isCurrentMonth: {
      type: Boolean,
      default: false
    },
    // isInInterval:{
    //     type:Boolean,
    //     default: false
    // },
    isToday: {
      type: Boolean,
      default: false
    },
    mycounter:{
      type:Number
    },
    interval:{
      type:Array
    },
    days:{
      type:Array
    }


    
    // selectedDate: {
    //   type: Object,
    //   required: true
    // }

  },

  computed: {
    label() {
      return dayjs(this.day.date).format("D");
    },
    disabledday(){
      if(!this.day.isCurrentMonth && this.day.weekend){
        return true
      }
      else if(this.day.weekend == true){
        return true
      }
      else{
        return false
      }
    }

  },

  methods: {

    start_end_date(){
      let start_end = dayjs(this.day.date); 
      this.$emit("start_end_selected",start_end)
      //console.log(this.interval,"xx")
       console.log("xxxx",this.interval)
      // console.log(this.days)
      // for(var i of this.days){
      //   for(var j of this.interval){
      //     this.intervalNow = false
      //     if(j.format("YYYY-MM-DD") == i.date){
      //        this.intervalNow = true            
      //     }
      //   }
      // }
      
      // for(var i of this.interval){
      //   console.log(i.format("YYYY-MM-DD"))
      // }
      // for(var j of this.days){
      //   console.log(j.date)
      // }
    }
  },
  printinerval(){

  }
};
</script>

<style scoped>
.calendar-day {
  position: relative;
  font-size: 16px;
  background-color: #fff;
  color: var(--grey-800);

}

.calendar-day > span {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;

}

.weekend{
  pointer-events: none !important;
  color: rgb(148, 145, 145);
}
.calendar-day--not-current {
  color: #3C3C3B;
  opacity: 0.1;
  
}
.interval-now{
  background-color: lightgreen ;
}
/* 
 */
.calendar-day--today {
  padding-top: 4px;
  font-size: 14px;
  font: normal normal normal 14px/27px Poppins;
  color: #3C3C3B;
}

/* .calendar-day--today > span {
  color: #fff;
  border-radius: 9999px;
  background-color: var(--grey-800);
  
/* }
 */
li{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 40px;
  
} 
li:hover{
  background-color: #3C3C3B0D;
  cursor: pointer;
}
li:focus{
  background-color: green ;
}
.int{
  background-color: green !important;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
}
.int2{
  background-color: green !important;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
}
.fulldate{
  background-color: green;
}
</style>
