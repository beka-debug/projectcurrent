<template>
   <div class="main">
    <div class="calendar-month">
      <PickerSelectDate
      @fulldateemitter="fulldtsubscriber"
      />
      <div class="calendar-month-header">
        <PickerDateIndicator
          :selected-date="selectedDate"
          @dateSelected="selectDate"
          class="calendar-month-header-selected-month"
          @showmonth="showmonthsubscriber"
        />
  
        <PickerDateSelector
          :current-date="today"
          :selected-date="selectedDate"
          @dateSelected="selectDate"
          
          @showYearFuncEmitter="showYearFuncSubscriber"
        />
      </div>
  
      <PickerWeekDay
      v-if ="!showDays"
      />
  
      <ol class="days-grid">
        <PickerMonthDayItem
        v-if ="!showDays"
          @choosedateemitter="choosedatesubscriber"
          v-for="day in days"
          :key="day.date"
          :day="day"
          :is-today="day.date === today"
        />
      </ol>
      <Years
      v-if="this.showyear"
      @chooseyearemitter="chooseYearSubscriber"
      />
      <Months
    @choosemonth = "chooseMonthSubscriber"
    
    v-if ="this.show"
    
    />
      
      </div>
      <div class="minutehours">
        <HourMinutes/>
        <TextHourMinutes/>
        <PickerHourMinutes
        @choosehouremitter="choosehoursubscriber"
        @chooseminuteemitter="chooseminutesubscriber"
        @scrollhouremitter="scrollhoursubscriber"
        />

      </div>

      {{selectedDate.format("DD-MM-YYYY-HH-mm")}}
    </div>
  </template>
  
  <script>
  import dayjs from "dayjs";
  import weekday from "dayjs/plugin/weekday";
  import weekOfYear from "dayjs/plugin/weekOfYear";
  import PickerMonthDayItem from "./PickerMonthDayItem";
  import PickerDateIndicator from "./PickerDateIndicator";
  import PickerDateSelector from "./PickerDateSelector";
  import PickerWeekDay from "./PickerWeekDay";
  import PickerSelectDate from "./PickerSelectDate";
  import PickerHourMinutes from "./PickerHourMinutes"
  import HourMinutes from "./HourMinutes"
  import TextHourMinutes from "./TextHourMinutes"
  import Years from "./Years"
  import Months from "./Months"
  
  dayjs.extend(weekday);
  dayjs.extend(weekOfYear);
  
  export default {
    name: "PickerMainMonth",
  
    components: {
        PickerMonthDayItem,
        PickerDateIndicator,
        PickerDateSelector,
        PickerWeekDay,
        PickerSelectDate,
        PickerHourMinutes,
        HourMinutes,
        TextHourMinutes,
        Years,
        Months
    },
  
    data() {
      return {
        selectedDate: dayjs(),
        showyear:false,
        show:false
      };
    },
  
    computed: {
      days() {
        return [
          ...this.previousMonthDays,
          ...this.currentMonthDays,
          ...this.nextMonthDays
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
        return [...Array(this.numberOfDaysInMonth)].map((day, index) => {
          let date = dayjs(`${this.year}-${this.month}-${index + 1}`).format("YYYY-MM-DD")
          return {
            date,
            isCurrentMonth: true,
            isSeletedDate: true ? this.selectedDate.format("YYYY-MM-DD") == date : falsen
          };
        });
      },
  
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
            let date = dayjs(`${previousMonth.year()}-${previousMonth.month()+1}-${previousMonthLastMondayDayOfMonth + index}`).format("YYYY-MM-DD")
            //console.log(date,"date")
            return {
                date,
                isCurrentMonth: false,
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
  
        return [...Array(visibleNumberOfDaysFromNextMonth)].map(
          (day, index) => {
          return {
            date: dayjs(
              `${nextMonth.year()}-${nextMonth.month() + 1}-${index + 1}`
            ).format("YYYY-MM-DD"),
            isCurrentMonth: false
          };
        });
      },

    showDays(){
     if(this.show ==  true || this.showyear == true){
      return true
     }
     else{
          return false
        }
      }
    },
  
    methods: {
      scrollhoursubscriber(e){
        this.selectedDate = (this.selectedDate.set('hour',e))
      },
      showYearFuncSubscriber(e){
      this.showyear = !this.showyear
      console.log(this.showyear)
    },
    showmonthsubscriber(){
      this.show = !this.show
      console.log(this.show)
    },
    chooseMonthSubscriber(mon){
      this.choosenmonth = mon.month;
      this.show = mon.show
      let choosenindex = mon.index + 1
      let currentindex = dayjs(this.selectedDate).format("MM") 
      let interv = currentindex - choosenindex
      this.selectedDate = dayjs(this.selectedDate).subtract(interv, "month");
      console.log("as")
    },
    chooseYearSubscriber(yr){
         this.choosenyear = yr.year
         this.showyear = false
         let interval = dayjs(this.selectedDate).format("YYYY") - yr.year
         this.selectedDate =  dayjs(this.selectedDate).subtract(interval, "year");
    },
      getWeekday(date) {
        return dayjs(date).weekday();
      },
  
      selectDate(newSelectedDate) {
        this.selectedDate = newSelectedDate;
      },
      choosedatesubscriber(e){
       this.selectedDate = dayjs(e)
      // console.log("aa")
    },
    fulldtsubscriber(e){
      this.selectedDate =  dayjs(e)
    },
    choosehoursubscriber(e){
        //this.selectedDate.hour() = 10
        //dayjs(this.selectedDate).set('month',3)
        //console.log(typeof(this.selectedDate))
        this.selectedDate = (this.selectedDate.set('hour',e))
        
       // console.log(this.selectedDate.format("MM"))
    },
    chooseminutesubscriber(e){
      this.selectedDate = (this.selectedDate.set('minute',e))
      console.log("ee")
    }
    },

  };
  </script>
  
  <style scoped>
  .calendar-month {
    position: relative;
    padding:15px 21px;
    padding-top: 10px;
    border:1px solid #3C3C3B1A;
    width: 230px !important;
    
  }
  
  .day-of-week {
    color: var(--grey-800);
    font-size: 18px;
    background-color: #fff;
    padding-bottom: 5px;
    padding-top: 10px;
  }
  
  .day-of-week,
  .days-grid {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    
    
  }
  .calendar-month-header{
    
    align-items: center;
  }
  .day-of-week > * {
    text-align: right;
    
  }
  
  .days-grid {
    height: 100%;
    position: relative;

  }
  .main{
    width: 772px;
    
    display: flex;
    align-items: flex-start;
  }
  .minutehours{
    padding-top: 10px;
    width: 130px;
  }
  </style>
  