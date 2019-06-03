<template>
  <div id="app">
    {{ yearAndMonth }}
    <button @click="goPrevMonth">prev</button>
    <button @click="goNextMonth">next</button>

    <table>
      <tr>
        <th>Sun</th>
        <th>Mon</th>
        <th>Tue</th>
        <th>Wed</th>
        <th>Thu</th>
        <th>Fri</th>
        <th>Sat</th>
      </tr>
      <template v-for="week in calendarData">
        <tr>
          <template v-for="day in week">
            <td>{{ day }}</td>
          </template>
        </tr>
      </template>
    </table>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "app",
  data: () => ({
    monthCounter: 0
  }),
  computed: {
    currentDate() {
      return moment().add(this.monthCounter, "months");
    },
    yearAndMonth() {
      return this.currentDate.format("YYYY MM");
    },
    calendarData() {
      // the value will be 28(days) if it's Fedurary for example
      const numOfDaysInMonth = this.currentDate.endOf("month").date();

      // create the same length of Array as number of the days in the month
      // -> [0,1,2,3...27], then make it [1,2,3...28]
      const daysArr = [...Array(numOfDaysInMonth).keys()].map(i => ++i);

      // if the first day in the month is Sunday, return 0. if Monday, return 1. if Saturday, 6.
      const indexOfTheFirstDay = this.currentDate.startOf("month").weekday();

      // create empty arrays for Calendar
      const weeksInCalendar = [...Array(6)];
      const daysInWeek = [...Array(7)];

      const data = weeksInCalendar.map((empty, weekIndex) =>
        daysInWeek.map((empty, dayIndex) => {
          const index = 7 * weekIndex + dayIndex - indexOfTheFirstDay;
          if (index < 0 || daysArr[index] === undefined) {
            return null;
          }
          return daysArr[index];
        })
      );

      // if there is an array of week with only null, remove it
      return data.filter(week => week.filter(day => day != null).length > 0);
    }
  },
  methods: {
    goNextMonth() {
      this.monthCounter++;
    },
    goPrevMonth() {
      this.monthCounter--;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>




