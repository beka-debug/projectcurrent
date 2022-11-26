<template>
    <div class="calendar-month">
      <SelectDate/>
      <div class="calendar-month-header">
        <DateIndicator1     
          :selected-date="selectedDate"
          class="calendar-month-header-selected-month"
          @dateSelected="selectDate"
          @showmonth="showmonthsubscriber"
        />
  
        <DateSelector1
          :current-date="today"
          :selected-date="selectedDate"
          @dateSelected="selectDate"
          @showYearFuncEmitter="showYearFuncSubscriber"
        />
      </div>
  
      <WeekDays1
      v-if ="!showDays"
      />
  
      <ol  
      v-if ="!showDays"
      class="days-grid">
        <MonthDayItem1
        
        @start_end_selected="start_end"
          
          
          :days="days"
          v-for="day in days"
          :key="day.date"
          :day="day"
          :is-today="day.date === today"
        />
      </ol>


      <Months1
    @choosemonth = "chooseMonthSubscriber"
    v-if ="this.show"
    :curmonths="curmonths"
    />
    <Years1
    v-if="this.showyear"
   @chooseyearemitter="chooseYearSubscriber"
    />
      <button @click="int()">int</button>
      {{selectedDate}}
    </div>
  
    
  </template>
  
  <script>
  import dayjs, { Dayjs } from "dayjs";
  import weekday from "dayjs/plugin/weekday";
  import weekOfYear from "dayjs/plugin/weekOfYear";
  import MonthDayItem1 from "./MonthDayItem1";
  import DateIndicator1 from "./DateIndicator1";
  import DateSelector1 from "./DateSelector1";
  import WeekDays1 from "./WeekDays1";
  import Months1 from "./Months1";
  import Years1 from "./Years1"
  import SelectDate from "./SelectDate";
//   import Months from "./Months";
  
  
  dayjs.extend(weekday);
  dayjs.extend(weekOfYear);
  var localeData = require('dayjs/plugin/localeData')
  dayjs.extend(localeData)
  
  dayjs().localeData()
  
  export default {
    name: "Month1",
  
    components: {
      MonthDayItem1,
      DateIndicator1,
      DateSelector1,
      WeekDays1,
      Months1,
      Years1,
      SelectDate
      //Months,
    },
    // mounted(){
    //   this.interval = this.leftinterval
    // },
  
    data() {
      return {
        selectedDate: dayjs(),
        selectedDatee: dayjs(),
        arr:[],
        interval:[],
        show:false,
        showyear:false,
        curmonths:dayjs.months(),
        choosenmonth:"",
        monthindex:0
      };
    },
    props:{
      startDate:dayjs(),
      leftinterval:[]
    },
    computed: {
      showDays(){
     if(this.show ==  true || this.showyear == true){
      return true
     }
     else{
      return false
     }
    },
      days() {
        return [
          ...this.previousMonthDays,
          ...this.currentMonthDays,
          ...this.nextMonthDays,
          // ...this.intervalDays
        ];
      },
      
  
      today() {
        return dayjs().format("YYYY-MM-DD");
      },
  
      month() {
        return Number(this.selectedDate.format("M"));
      },
  
      year() {
        return Number(this.selectedDate.format("YYYY"));
      },
  
      numberOfDaysInMonth() {
        return dayjs(this.selectedDate).daysInMonth();
      },
  
      currentMonthDays() {
        let intarr = []
        // for(var i of this.interval){
        //   let obj = {
        //     date:i.format("YYYY-MM-DD"),
        //     isInterval:true,
        //     isCurrentMonth: true,
        //   }
        //   intarr.push(obj)
          
        // }
       // console.log(this.interval)
        for(var i of this.leftinterval){
          intarr.push(i.format("YYYY-MM-DD"))
        }
        let firstInterval = intarr[0];
      let lastInterval = intarr[intarr.length-1];
       
        //console.log(intarr)
        return [...Array(this.numberOfDaysInMonth)].map((day, index) => {
          let date = dayjs(`${this.year}-${this.month}-${index + 1}`).format(
              "YYYY-MM-DD")
          return {   
            date,
            isCurrentMonth: true,
           isInterval: true ? intarr.includes(date) : false,
           clicked:true,
           first:true ? date == firstInterval : false,
         last:true ? date == lastInterval : false,
          };
        });
      },
      // intervalDays(){
  
      //   let intarr = []
      //   for(var i of this.interval){
      //     let obj = {
      //       date:i.format("YYYY-MM-DD"),
      //       isInterval:true,
      //       isCurrentMonth: true,
      //     }
      //     intarr.push(obj)
      //   }
      //   return intarr
      // },
      previousMonthDays() {
        const firstDayOfTheMonthWeekday = this.getWeekday(
          this.currentMonthDays[0].date
        );
        const previousMonth = dayjs(`${this.year}-${this.month}-01`).subtract(
          1,
          "month"
        );
  
        // Cover first day of the month being sunday (firstDayOfTheMonthWeekday === 0)
        const visibleNumberOfDaysFromPreviousMonth = firstDayOfTheMonthWeekday
          ? firstDayOfTheMonthWeekday - 1
          : 6;
  
        const previousMonthLastMondayDayOfMonth = dayjs(
          this.currentMonthDays[0].date
        )
          .subtract(visibleNumberOfDaysFromPreviousMonth, "day")
          .date();
  
        return [...Array(visibleNumberOfDaysFromPreviousMonth)].map(
          (day, index) => {
            return {
              date: dayjs(
                `${previousMonth.year()}-${previousMonth.month() +
                  1}-${previousMonthLastMondayDayOfMonth + index}`
              ).format("YYYY-MM-DD"),
              isCurrentMonth: false
            };
          }
        );
      },
  
      nextMonthDays() {
        const lastDayOfTheMonthWeekday = this.getWeekday(
          `${this.year}-${this.month}-${this.currentMonthDays.length}`
        );
  
        const nextMonth = dayjs(`${this.year}-${this.month}-01`).add(1, "month");
  
        const visibleNumberOfDaysFromNextMonth = lastDayOfTheMonthWeekday
          ? 7 - lastDayOfTheMonthWeekday
          : lastDayOfTheMonthWeekday;
  
        return [...Array(visibleNumberOfDaysFromNextMonth)].map((day, index) => {
          return {
            date: dayjs(
              `${nextMonth.year()}-${nextMonth.month() + 1}-${index + 1}`
            ).format("YYYY-MM-DD"),
            isCurrentMonth: false,
            clicked:true
          };
        });
        
      }
    },
  
    methods: {
      showYearFuncSubscriber(e){
      this.showyear = !this.showyear
      console.log("dasd")
    },
      chooseMonthSubscriber(mon){
        this.choosenmonth = mon.month;
        this.show = mon.show
        let choosenindex = mon.index + 1
        let currentindex = dayjs(this.selectedDate).format("MM") 
        // if(choosenindex < currentindex){
          let interv = currentindex - choosenindex
          this.selectedDate = dayjs(this.selectedDate).subtract(interv, "month");
        //}
        // console.log(this.choosenmonth,"subscribed", this.curmonths[this.index])
      },
      chooseYearSubscriber(yr){
         this.choosenyear = yr.year
         this.showyear = false
         let interval = dayjs(this.selectedDate).format("YYYY") - yr.year
         //console.log(interval)
         this.selectedDate =  dayjs(this.selectedDate).subtract(interval, "year");
       // console.log(this.selectDate)
       // console.log(dayjs(this.selectedDate).format("YYYY"))
    },
      int(){
      console.log(this.leftinterval)
      },
      showmonthsubscriber(x){
               this.show = x
          },
      getWeekday(date) {
        return dayjs(date).weekday();
      },
  
      selectDate(newSelectedDate) {
        this.selectedDate = newSelectedDate;
      },
      selectDatee(newSelectedDate){
        this.selectedDatee = newSelectedDate;
      },
      
      start_end(starEndDate){
        console.log(dayjs(this.startDate),"++++++++++")
        this.interval = this.leftinterval
       this.arr.push(dayjs(this.startDate).format("YYYY-MM-DD"))
        //console.log(this.selectedDate)
        //console.log(this.startDate)
         this.selectedDate = starEndDate
         
         this.arr.push(this.selectedDate.format("YYYY-MM-DD"))
         this.$emit("secondintervalemitter",this.arr[1])
         if(this.arr.length > 1){
          
          //console.log(this.selectedDate.diff(this.arr[this.arr.length - 1]),"days")
          //console.log(dayjs(this.arr[1]))
          let date1 = dayjs()
          if(this.arr.length > 2){
            date1 = dayjs(this.arr[2])
          } 
          else{
            date1 = dayjs(this.arr[1])
          }   
          //this.startDate = dayjs(this.arr[this.arr.length - 2])
         // console.log(this.startDate, date1)
        //  console.log(date1,dayjs(this.startDate.format("YYYY-MM-DD"),"fsgfgdfg"))
          //console.log(this.arr)
          let diff = date1.diff(this.startDate,"hours")/24
          //console.log(diff)
          //console.log(date1)
          //console.log(this.startDate)
          if(this.arr.length > 2){
            // console.log(this.arr[2],this.arr)
            //this.$emit("lastintervalemitter",this.arr[2])
          }
          else{
            //console.log(typeof(this.arr[1]),this.arr)
            this.$emit("lastintervalemitter",this.arr[1])
          }
          //this.$emit("newarremitter",this.arr)
          // let interval = []
          //console.log(this.arr)
         // console.log(this.date1)
          //dayjs(this.selectedDate).subtract(1, "year");
          // console.log(date2.daysInMonth())
          for(let i = 0; i <= diff;i++){
            this.interval.push(dayjs(this.startDate).add(i,"day"))
            
          }
          
          this.arr = this.arr.reverse().splice(0,1)
          
          //console.log(this.interval)
          
          //console.log(this.intervalDays)
          //console.log(this.currentMonthDays)
          
          // for(let i of this.days){
             
          //   currentdays.push(i.date)
  
          // }
          let x = 0
        // for(let i of this.interval){
        //   for(let j of this.days){
        //     if(dayjs(i).format("YYYY-MM-DD") == j.date){
               
        //     } 
        //   }
        // }
        // this.$emit("intervals",interval)
        // გავატანოთ ინფორმაცია ინტერვალზე და 
        // მიმდინარე დღეებზე მოცემულ თვეში
        // https://www.youtube.com/watch?v=UtV-WZUtak4
          
         }
        },
      // y(){
      //   this.arr.push(this.selectedDate.format("YYYY-MM-DD"))
      //   console.log(this.arr)
      //   if(this.arr.length > 1){
      //    console.log(this.arr[this.arr.length - 1], this.arr[this.arr.length-2])
      //   }
      // }
  
    }
  };
  </script>
  
  <style scoped>
  .calendar-month {
    position: relative;
    background-color: var(--grey-200);
    border: solid 1px var(--grey-300);
    width:300px;
    padding: 10px 0px;
    background-color: transparent;
  
    
  }
  
  .day-of-week {
    color: #3C3C3B;
    font-size: 14px;
    background-color: #fff;
    font-family: normal normal normal 14px/27px Poppins;
    padding-top: 10px;
  }
  
  .day-of-week, .days-grid {
    display: grid;
    grid-template-columns: repeat(7,1fr);
    border:none !important;
    padding: 5px 15px;
    
  }
  
  .day-of-week > * {
    text-align: right;
    
  }
  
  .days-grid {
    height: 100%;
    position: relative;
  
    border:none !important;
  }
  
  .calendar-month-header{
    border-bottom: 1px solid gray;  
    padding: 5px 15px;
    display: flex;
    align-items: center;
  }
  </style>
  