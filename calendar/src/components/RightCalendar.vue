<template>
    <div>
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
    </div>
</template>

<script>

import CalendarDateIndicator from "./CalendarDateIndicator";
import CalendarDateSelector from "./CalendarDateSelector";
import CalendarWeekdays from "./CalendarWeekdays";

components: {

    CalendarDateIndicator,
    CalendarDateSelector,
    CalendarWeekdays
    }
export default{
    
}
</script>

<style scoped>

</style>