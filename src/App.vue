<template>
  <div id="app">
    {{ yearMonth }}
    <button @click="goPrevMonth">prev</button>
    <button @click="goNextMonth">next</button>

    <table>
      <tr>
        <th>日</th>
        <th>月</th>
        <th>火</th>
        <th>水</th>
        <th>木</th>
        <th>金</th>
        <th>土</th>
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
    current: 0
  }),
  computed: {
    currentMoment() {
      return moment().add(this.current, "months");
    },
    yearMonth() {
      return this.currentMoment.format("YYYY MM");
    },
    calendarData() {
      // この月に何日まであるかを算出
      const numOfMonth = this.currentMoment.endOf("month").date();
      // この月の1日〜最終日までの配列
      const daysOfMonth = [...Array(numOfMonth).keys()].map(i => ++i);
      // 1日の曜日（0~6の数値で取得）
      const firstWeekDay = this.currentMoment.startOf("month").weekday();
      // 週ごとの二次元配列を生成
      const data = [...Array(6)].map((empty, weekIndex) =>
        [...Array(7)].map((empty, dayIndex) => {
          const i = 7 * weekIndex + dayIndex - firstWeekDay;
          if (i < 0 || daysOfMonth[i] === undefined) {
            return null;
          }
          return daysOfMonth[i];
        })
      );
      console.log(data);
      // 全てnullの配列があれば除去する
      return data.filter(week => week.filter(day => day != null).length > 0);
    }
  },
  methods: {
    goNextMonth() {
      this.current++;
    },
    goPrevMonth() {
      this.current--;
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
