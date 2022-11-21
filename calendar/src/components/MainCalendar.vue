<template>
    <div class="main">
        <div class="leftcalendar">
    <CalendarMonth
    :endDate="endDate"
    :newint="newint"
    @leftintervalemitter="leftintervalsubscriber"
    @intervalemitter="intervalSubscriber"></CalendarMonth>
</div>
<div class="rightcalendar">
    <Month1 
    :startDate="startDate"
    :leftinterval="leftinterval"
    @secondintervalemitter="secondintervalSubscriber"
    ></Month1>
</div>
    <button @click="alldates">dates</button>
    <!-- <CalendarMonth></CalendarMonth> -->

    <!-- <RightCalendar></RightCalendar> -->
    <!-- <CalendarMonth></CalendarMonth> -->
</div>
    
</template>

<script>
import dayjs from 'dayjs';
import { computed } from 'vue';
import CalendarMonth from './CalendarMonth';
import Month1 from './Month1';
export default{
   components:{
       CalendarMonth,
       Month1

   },
   data() {
    return {
        startDate:dayjs(),
        endDate:dayjs(),
        newint:[],
        counter:0,
        leftinterval:[]
    }
   },
   computed:{
    end(){
        return this.startDate
    }
   },
//    props:{
//       startdate:String
//    },
   methods: {
    intervalSubscriber(dt){
        this.startDate = dt
       // console.log(this.endDate,"------------------")
       this.newint.push(this.startDate)
       
    },
    secondintervalSubscriber(dt){
       this.endDate = dt
       this.newint.push(this.endDate)
    },
    alldates(){
        console.log(this.startDate)
        console.log(this.endDate)
    },
    leftintervalsubscriber(leftint){
        this.leftinterval = leftint
        console.log(this.leftinterval)
    }
},
}
</script>

<style>
.main{
    display: flex;
    align-items: center;
}
</style>