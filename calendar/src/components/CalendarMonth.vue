<template>
  <div class="calendar-month">
    <div class="left">
    <div class="calendar-month-header">
      <CalendarDateIndicator     
        :selected-date="selectedDate"
        class="calendar-month-header-selected-month"
        @dateSelected="selectDate"
        @showmonth="showmonthsubscriber"
      />

      <CalendarDateSelector
        :current-date="today"
        :selected-date="selectedDate"
        @dateSelected="selectDate"
      />
    </div>

    <CalendarWeekdays/>

    <ol  class="days-grid">
      <CalendarMonthDayItem
      
      @start_end_selected="start_end"
        :interval="interval"
        :days="days"
        v-for="day in days"
        :key="day.date"
        :day="day"
        :is-today="day.date === today"
      />
    </ol>
    {{selectedDate}}
    <Months
    @choosemonth = "chooseMonthSubscriber"
    v-if ="this.show"
    :curmonths="curmonths"
    />
  </div>
  <div class="right">
    <div class="calendar-month-header">
      <RightCalendarDateIndicator     
        :selected-datee="selectedDatee"
        class="calendar-month-header-selected-month"
        @dateSelected="selectDatee"
        @showmonth="showmonthsubscriber"
      />

      <CalendarDateSelector
        :current-date="today"
        :selected-date="selectedDate"
        @dateSelected="selectDate"
      />
    </div>

    <CalendarWeekdays/>

    <ol  class="days-grid">
      <CalendarMonthDayItem
      
      @start_end_selected="start_end"
        :interval="interval"
        :days="days"
        v-for="day in days"
        :key="day.date"
        :day="day"
        :is-today="day.date === today"
      />
    </ol>
    {{selectedDate}}
    <Months
    @choosemonth = "chooseMonthSubscriber"
    v-if ="this.show"
    :curmonths="curmonths"
    />
  </div>
  </div>
  
</template>

<script>
import dayjs from "dayjs";
import weekday from "dayjs/plugin/weekday";
import weekOfYear from "dayjs/plugin/weekOfYear";
import CalendarMonthDayItem from "./CalendarMonthDayItem";
import CalendarDateIndicator from "./CalendarDateIndicator";
import CalendarDateSelector from "./CalendarDateSelector";
import CalendarWeekdays from "./CalendarWeekdays";
import Months from "./Months";
import RightCalendarDateIndicator from "./RightCalendarDateIndicator";

dayjs.extend(weekday);
dayjs.extend(weekOfYear);
var localeData = require('dayjs/plugin/localeData')
dayjs.extend(localeData)

dayjs().localeData()

export default {
  name: "CalendarMonth",

  components: {
    CalendarMonthDayItem,
    CalendarDateIndicator,
    CalendarDateSelector,
    CalendarWeekdays,
    Months,
    RightCalendarDateIndicator
  },
//   mounted(){
//    console.log(dayjs.months())
// },

  data() {
    return {
      selectedDate: dayjs(),
      selectedDatee: dayjs(),
      arr:[],
      interval:[],
      show:false,
      curmonths:dayjs.months(),
      choosenmonth:"",
      monthindex:0
    };
  },

  computed: {
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
      console.log(this.interval)
      for(var i of this.interval){
        intarr.push(i.format("YYYY-MM-DD"))
      }
     
     
      console.log(intarr)
      return [...Array(this.numberOfDaysInMonth)].map((day, index) => {
        let date = dayjs(`${this.year}-${this.month}-${index + 1}`).format(
            "YYYY-MM-DD")
        return {   
          date,
          isCurrentMonth: true,
         isInterval: true ? intarr.includes(date) : false,
         clicked:true
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
    chooseMonthSubscriber(mon){
      this.choosenmonth = mon.month;
      this.show = mon.show
      let choosenindex = mon.index + 1
      let currentindex = dayjs(this.selectedDate).format("MM") 
      if(choosenindex < currentindex){
        let interv = currentindex - choosenindex
        this.selectedDate = dayjs(this.selectedDate).subtract(interv, "month");
      }
      // console.log(this.choosenmonth,"subscribed", this.curmonths[this.index])
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
      this.interval = []
       this.selectedDate = starEndDate
       this.arr.push(this.selectedDate.format("YYYY-MM-DD"))
       if(this.arr.length > 1){
        
        //console.log(this.selectedDate.diff(this.arr[this.arr.length - 1]),"days")
        let date1 = dayjs(this.arr[this.arr.length - 1])
        let date2 = dayjs(this.arr[this.arr.length - 2])
        console.log(date1,date2)
        console.log(this.arr)
        let diff = date1.diff(date2,"hours")/24
        // let interval = []
        let currentdays = []
        //dayjs(this.selectedDate).subtract(1, "year");
        // console.log(date2.daysInMonth())
        for(let i = 0; i < diff;i++){
          this.interval.push(date2.add(i,"day"))
          
        }
        this.arr = this.arr.reverse().splice(0,1)
        
        //console.log(this.interval)
        
        console.log(this.intervalDays)
        console.log(this.currentMonthDays)
        
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
  width:600px;
  padding: 10px 30px;
  background-color: transparent;
  display: flex;
  justify-content: space-between;
  
}
.left{
  
}
.left, .right{
  width: 280px;
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
  
}

.day-of-week > * {
  text-align: right;
  
}

.days-grid {
  height: 100%;
  position: relative;

  border:none !important;
}
li{
  
}

</style>
