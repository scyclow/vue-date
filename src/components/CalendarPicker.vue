<template>
  <div class="calendar-picker">
    <div class="date-header">
      <div class="date-year">{{ year }}</div>
      <div class="month-selector">
        <span class="switch-month" v-on:click="updateMonth(-1)">&#60</span>
        <span>{{ month }}</span>
        <span class="switch-month" v-on:click="updateMonth(1)">&#62</span>
      </div>
    </div>

    <div class="month-days">
      <table>
        <tr v-for="week in monthWeeks">
          <td v-for="day in week" v-on:click="updateDay(day)">{{ day }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>

import moment from 'moment';
import _ from 'lodash';
window.moment = moment


function getDaysOfWeek(date = moment()) {
  const startingDayOfMonth = date.date(1).day();
  const endingDateOfMonth = date.add(1, 'month').subtract(1, 'day').date();

  const weeks = [[]];

  let currentDay = 1;

  // Make sure the first day of the month starts off as the correct day of week.
  weeks[0][startingDayOfMonth] = currentDay;
  currentDay++;

  while (currentDay <= endingDateOfMonth) {
    let currentWeek = _.last(weeks);

    if (currentWeek.length === 7) {
      currentWeek = [];
      weeks.push(currentWeek);
    }

    currentWeek.push(currentDay)

    currentDay++
  }

  return weeks;
}

export default {
  name: 'calendarPicker',

  props: ['onChange'],

  data() {
    return {
      date: moment()
    }
  },

  computed: {
    month() {
      return this.date.format('MMMM')
    },
    year() {
      return this.date.year()
    },
    monthWeeks() {
      return getDaysOfWeek(this.date);
    },
  },

  methods: {
    updateMonth(direction) {
      // create a new moment object so it registers vue's change detection.
      this.date = moment(this.date.add(direction, 'month'))
    },
    updateDay(day) {
      this.onChange(this.date.date(day))
    }
  }
}

</script>

<style scoped>

.calendar-picker {
  width: 300px;
  border: 2px solid #777;
  border-radius: 4px;
  padding: 15px;
  background-color: #fff;
}

.date-year {
  width: 100%;
  text-align: center;
}

.month-selector {
  display: flex;
  justify-content: space-between;
}

.switch-month {
  cursor: pointer;
  user-select: none;
  color: #777;
  font-size: 20px;
  font-weight: 800;
}

.switch-month:active {
  color: #ccc;
}

.month-days {
  font-size: 25px;
}

.month-days table {
  margin: auto;
}

.month-days td {
  padding: 5px;
  cursor: pointer;
}

.month-days td:hover {
  background-color: #bbb;
  user-select: none;
}

</style>
