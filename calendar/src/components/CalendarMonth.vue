<template>
  <div class="calendar-month">
    <selectDate
    @fulldateemitter="fulldatesubscriber"
    />
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
        @showYearFuncEmitter="showYearFuncSubscriber"
      />
    </div>

    <CalendarWeekdays
    v-if ="!showDays"
    />

    <ol 
    v-if ="!showDays"
    class="days-grid">
      <CalendarMonthDayItem
      

      @start_end_selected="start_end"
        
        :days="days"
        v-for="day in days"
        :key="day.date"
        :day="day"
        :is-today="day.date === today"
      />
    </ol>
    
    <Months
    @choosemonth = "chooseMonthSubscriber"
    v-if ="this.show"
    :curmonths="curmonths"
    />

    <Years
    v-if="this.showyear"
   @chooseyearemitter="chooseYearSubscriber"
    />
    
    <button @click="fillDropYears()">xxxx</button>

    {{selectedDate}}
  </div>

  
</template>

<script>
import dayjs, { Dayjs } from "dayjs";
import weekday from "dayjs/plugin/weekday";
import weekOfYear from "dayjs/plugin/weekOfYear";
import CalendarMonthDayItem from "./CalendarMonthDayItem";
import CalendarDateIndicator from "./CalendarDateIndicator";
import CalendarDateSelector from "./CalendarDateSelector";
import CalendarWeekdays from "./CalendarWeekdays";
import Months from "./Months";
import Years from "./Years";
import SelectDate from "./SelectDate";


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
    Years,
    SelectDate
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
      showyear:false,
      curmonths:dayjs.months(),
      choosenmonth:"",
      choosenyear:null,
      monthindex:0,
      dropYears:[],
      fulldate:dayjs(),
      daysInCurrentMonth:[],
      x:"",
      date1:null
    };
  },
  props:{
      endDate:dayjs(),
      newint:[],
      lastinterva:""
      
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
    z(){
              if(this.lastinterva != ""){
        if(this.arr[this.arr.length - 1]>this.lastinterva){
          this.date1 = dayjs(this.lastinterva)
          console.log("last", this.lastinterva)
        }
        else{
          this.date1 = dayjs(this.arr[this.arr.length - 1])
          console.log("same", this.lastinterva)
        }
      }
      else{
        this.date1 = dayjs(this.arr[this.arr.length - 1])
        console.log("empty first")
      }
      return this.date1
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
      //console.log(this.interval)
      for(var i of this.interval){
        intarr.push(i.format("YYYY-MM-DD"))
      }
      let firstInterval = intarr[0];
      let lastInterval = intarr[intarr.length-1];
    // console.log(firstInterval,lastInterval)

    //  for(let i = 0; i < this.currentMonthDays.length; i++){
    //   console.log(this.currentMonthDays[i])
    //  }
     
      //console.log(intarr)
      let isWeekend = false
      return [...Array(this.numberOfDaysInMonth)].map((day, index) => {
        let date = dayjs(`${this.year}-${this.month}-${index + 1}`).format(
            "YYYY-MM-DD")
        let dt = new Date(`${this.year}-${this.month}-${index + 1}`)
        if(dt.getDay() == 6 || dt.getDay() == 0 ){
            isWeekend = true
        }
        else{
          isWeekend = false
        }
        return {   
          date,
          isCurrentMonth: true,
         isInterval: true ? intarr.includes(date) : false,
         clicked:true,
         first:true ? date == firstInterval : false,
         last:true ? date == lastInterval : false,
         weekend: isWeekend,
         fulldate: true ? (this.selectedDate == date) : false,
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
      let isWeekend = false
      return [...Array(visibleNumberOfDaysFromPreviousMonth)].map(
        (day, index) => {
          let date = dayjs(
              `${previousMonth.year()}-${previousMonth.month() +
                1}-${previousMonthLastMondayDayOfMonth + index}`
            ).format("YYYY-MM-DD")
            let dt = new Date(`${this.year}-${this.month}-${index + 1}`)
        if(dt.getDay() == 6 || dt.getDay() == 0 ){
            isWeekend = true
        }
        else{
          isWeekend = false
        }
          return {
            date,
            isCurrentMonth: false,
            weekend: isWeekend

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
    fulldatesubscriber(e){
      this.interval = []
      let dt = e

     this.fulldate = dayjs(`${dt.slice(6,10)}-${dt.slice(3,5)}-${dt.slice(0,2)}`)
     this.selectedDate = this.fulldate
     this.arr.push(this.selectedDate.format("YYYY-MM-DD"))
    this.$emit("intervalemitter",this.selectedDate.format("YYYY-MM-DD"))
    console.log("xxx")
    if(this.arr.length == 1){
        let date1 = dayjs(this.arr[this.arr.length - 1])
        let date2 = dayjs(this.lastinterva)
        
        let diff = date1.diff(date2,"hours")/24
        for(let i = 0; i <= diff;i++){
          this.interval.push(date2.add(i,"day"))
          
        }
        this.$emit("leftintervalemitter",this.interval)
        this.arr = this.arr.reverse().splice(0,1)
       }
       if(this.arr.length > 1){
        
        //console.log(this.selectedDate.diff(this.arr[this.arr.length - 1]),"days")
        let date1 = dayjs(this.arr[this.arr.length - 1])
        let date2 = dayjs(this.arr[this.arr.length - 2])
        console.log(date2)
        let diff = date1.diff(date2,"hours")/24

        for(let i = 0; i <= diff;i++){
          this.interval.push(date2.add(i,"day"))
          
        }
        this.$emit("leftintervalemitter",this.interval)
        this.arr = this.arr.reverse().splice(0,1)
        
       }
       let currmonth = this.selectedDate.format("MM")
      let curryear= this.selectedDate.format("YYYY")
      
      for(let i = 1; i <= this.selectedDate.daysInMonth();i++){
          this.daysInCurrentMonth.push(`${curryear}-${currmonth}-${i}`)
      }
     console.log("done")

    },
    showYearFunc(){
      this.showyear = true
      console.log("x")
    },
    showYearFuncSubscriber(e){
      this.showyear = !this.showyear
    },
    fillDropYears(){
      let dt = new Date().getFullYear()
        for(let i = dt - 11; i <= dt; i++){
            this.dropYears.push(i)
        }
        //this.$emit("dropyearsemitter", this.dropYears)
      // console.log(this.lastinterva)

      console.log(this.x)
       // console.log(this.dropYears, this.showyear)
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
    chooseMonthSubscriber(mon){
      this.choosenmonth = mon.month;
      this.show = mon.show
      let choosenindex = mon.index + 1
      let currentindex = dayjs(this.selectedDate).format("MM") 
      //if(choosenindex < currentindex){
        let interv = currentindex - choosenindex
        this.selectedDate = dayjs(this.selectedDate).subtract(interv, "month");
      //}
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
      this.$emit("intervalemitter",this.selectedDate.format("YYYY-MM-DD"))
      // if(this.arr.length == 1){

      // }
      console.log(this.arr)
       if(this.arr.length == 1){
        let date1 = dayjs(this.arr[this.arr.length - 1])
        let date2 = dayjs(this.lastinterva)
        let diff = date1.diff(date2,"hours")/24
        //this.interval = []
        for(let i = 0; i <= diff;i++){
          this.interval.push(date2.add(i,"day"))
          
        }
        this.$emit("leftintervalemitter",this.interval)

        this.arr = this.arr.reverse().splice(0,1)
       }
        if(this.arr.length > 1){
      //   //this.interval = []
      //   //console.log(this.selectedDate.diff(this.arr[this.arr.length - 1]),"days")
      //   if(this.lastinterva != ""){
      //   if(this.arr[this.arr.length - 1]>this.lastinterva){
      //     var date1 = dayjs(this.lastinterva)
      //     console.log("last", this.lastinterva)
      //   }
      //   else{
      //     var date1 = dayjs(this.arr[this.arr.length - 1])
      //     console.log("same", this.lastinterva)
      //   }
      // }
      // else{
      //   var date1 = dayjs(this.arr[this.arr.length - 1])
      //   console.log("empty first")
      // }
        
        console.log(this.lastinterva)
        console.log(this.z)
        //let x1 = dayjs(this.lastinterva).format("DD") 
        //let x2 = (dayjs(this.arr[this.arr.length - 2]).format("DD"))
       // this.date1 = dayjs(this.arr[this.arr.length -1])
        let date2 =  dayjs(this.arr[this.arr.length - 2])
        // console.log(this.arr[this.arr.length - 1],this.lastinterva)
        // console.log(this.arr[this.arr.length - 1]==this.lastinterva)
        //date2 = this.lastinterva
       // let date2 = this.lastinterva
        let diff = this.z.diff(date2,"hours")/24

        for(let i = 0; i <= diff;i++){
          this.interval.push(date2.add(i,"day"))
          
        }
        this.$emit("leftintervalemitter",this.interval)
        this.arr = this.arr.reverse().splice(0,1)
        
       }
      // დაემიტება start date-ის შესაცვლელად this.$emit()
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
