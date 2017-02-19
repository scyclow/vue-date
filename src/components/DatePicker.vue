<template>
  <div class="date-picker">
    <input placeholder="yyyy/mm/dd" :value="prettyDate" />
    <button v-on:click="toggleDate">Choose Date</button>
    <calendar-picker
      v-if="displayCalendar"
      class="calendar-picker"
      :on-change="updateDate"
    ></calendar-picker>
  </div>
</template>

<script>

import moment from 'moment';
import _ from 'lodash';


import CalendarPicker from './CalendarPicker'

export default {
  name: 'datePicker',
  components: {
    CalendarPicker
  },

  data() {
    return {
      displayCalendar: false,
      date: undefined
    }
  },

  computed: {
    prettyDate() {
      return this.date && this.date.format('YYYY/MM/DD')
    }
  },

  methods: {
    toggleDate() {
      this.displayCalendar = !this.displayCalendar
    },

    updateDate(date) {
      this.date = moment(date);
      this.displayCalendar = false;
    }
  }
}

</script>

<style scoped>
.date-picker {
  display: inline-flex;
  justify-content: center;
  width: 300px;
}

.date-picker input {
  padding: 5px;
  margin-right: 10px;
}

.date-picker button {
  cursor: pointer;
}

.date-picker button,
.date-picker input {
  font-size: 15px;
}

.calendar-picker {
  margin-top: 40px;
  position: absolute;
  z-index: 10;
  background-color: blue;
}


</style>
