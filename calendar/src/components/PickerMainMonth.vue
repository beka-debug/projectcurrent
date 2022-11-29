<template>
  
    <div class="calendar-month">
      <PickerSelectDate
      @fulldateemitter="fulldtsubscriber"
      />
      <div class="calendar-month-header">
        <PickerDateIndicator
          :selected-date="selectedDate"
          @dateSelected="selectDate"
          class="calendar-month-header-selected-month"
        />
  
        <PickerDateSelector
          :current-date="today"
          :selected-date="selectedDate"
          @dateSelected="selectDate"
        />
      </div>
  
      <PickerWeekDay/>
  
      <ol class="days-grid">
        <PickerMonthDayItem
          @choosedateemitter="choosedatesubscriber"
          v-for="day in days"
          :key="day.date"
          :day="day"
          :is-today="day.date === today"
        />
      </ol>
      <PickerHourMinutes/>
      {{selectedDate.format("YYYY-MM-DD")}}
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
        PickerHourMinutes
    },
  
    data() {
      return {
        selectedDate: dayjs()
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
            console.log(date,"date")
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
      }
    },
  
    methods: {
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
    }
    },

  };
  </script>
  
  <style scoped>
  .calendar-month {
    position: relative;
    padding: 10px;
    border: solid 1px var(--grey-300);
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
    display: grid;
    grid-template-columns: repeat(7, 1fr);
  }
  
  .day-of-week > * {
    text-align: right;
    background-color: red;
  }
  
  .days-grid {
    height: 100%;
    position: relative;
    grid-column-gap: var(--grid-gap);
    grid-row-gap: var(--grid-gap);
    border-top: solid 1px var(--grey-200);
  }
  </style>
  