<template>
  <div class="calendar-date-selector">
    <span @click="selectPrevious"><</span>
    <span @click="selectCurrent">{{selectedYear}}</span>
    <span @click="selectNext">></span>
    
  </div>

</template>

<script>
import dayjs from "dayjs";

export default {
  name: "CalendarModeSelector",

  props: {
    currentDate: {
      type: String,
      required: true
    },

    selectedDate: {
      type: Object,
      required: true
    }

  },
  computed:{
    selectedYear(){
      return this.selectedDate.format("YYYY");
    }
  },

  methods: {
    selectPrevious() {
      let newSelectedDate = dayjs(this.selectedDate).subtract(1, "year");
      this.$emit("dateSelected", newSelectedDate);
    },

    selectCurrent() {
      let newSelectedDate = dayjs(this.currentDate);
      this.$emit("dateSelected", newSelectedDate);
      this.$emit("showYearFuncEmitter",true)
    },

    selectNext() {
      let newSelectedDate = dayjs(this.selectedDate).add(1, "year");
      this.$emit("dateSelected", newSelectedDate);
    }
  }
};
</script>

<style scoped>
.calendar-date-selector {
  display: flex;
  justify-content: space-between;
  width: 80px;
  color: var(--grey-800);
}

.calendar-date-selector > * {
  cursor: pointer;
  user-select: none;
}
</style>
