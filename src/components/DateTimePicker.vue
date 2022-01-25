<template>
  <div class="date-picker" @click="toggleDatePicker($event)">
        <div class="selected-date" :data-value="selectedDateValue">{{ selectedDate }}</div>
        <div class="dates" ref="dates">
            <div class="month">
                <div class="arrows prev-mth" @click="goToPrevMonth">&lt;</div>
                <div class="mth">{{ currentMonth }}</div>
                <div class="arrows next-mth" @click="goToNextMonth">&gt;</div>
            </div>

            <div class="datepicker__week">
              <div v-for="weekDay in weekDays" :key="weekDay" class="datepicker__weekday">
                {{ weekDay }}
              </div>
            </div>

            <div class="datepicker__days">
              <div class="datepicker__day" :style="{width: getWeekStart() * 41 + 'px'}"></div>
              <div
                  v-for="day in amountDays" :key="day"
                  class="datepicker__day"
                  v-bind:class="selectedDay === (day+1) && selectedYear === year && selectedMonth === month ? 'selected' : ''"
                  @click="onSelectDate(day)">
                {{ day }}
              </div>
            </div>
        </div>
    </div>
</template>

<script>
import moment from 'moment';
export default {
  name: 'DateTimePicker',
  props: {
    msg: String
  },
  mounted() {
      let date = new Date();
      let day = date.getDate();
      this.month = date.getMonth();
      this.year = date.getFullYear();
      this.selectedDay = day+1;
      this.selectedMonth = this.month;
      this.selectedYear = this.year;
      this.currentMonth = this.months[this.month] + ' ' + this.year;
      this.selectedDate = this.formatDate(date);

      this.populateDates();

  },
  methods: {
    populateDates() {
      this.amountDays = new Date(this.year, (this.month+1), 0).getDate();

    },
    formatDate(d) {
      let day = d.getDate();
      if (day < 10) {
        day = '0' + day;
      }

      let month = d.getMonth() + 1;
      if (month < 10) {
        month = '0' + month;
      }

      let year = d.getFullYear();
      return day + ' / ' + month + ' / ' + year;
    },
    onSelectDate(i) {
      const date = new Date(this.year + '-' + (this.month + 1) + '-' + i);
      this.selectedDay = (i + 1);
      this.selectedMonth = this.month;
      this.selectedYear = this.year;

      this.selectedDate = this.formatDate(date);
      this.selectedDateValue = date;
    },
    goToNextMonth () {
      this.month++;
      if (this.month > 11) {
        this.month = 0;
        this.year++;
      }
      this.currentMonth = this.months[this.month] + ' ' + this.year;
      this.populateDates();
    },
    goToPrevMonth () {
      this.month--;
      if (this.month < 0) {
        this.month = 11;
        this.year--;
      }
      this.currentMonth = this.months[this.month] + ' ' + this.year;
      this.populateDates();
    },
    toggleDatePicker (e) {
      if (!this.checkEventPathForClass(e.path, 'dates')) {
      this.$refs.dates.classList.toggle('active');
      }
    },
    checkEventPathForClass (path, selector) {
      for (let i = 0; i < path.length; i++) {
        if (path[i].classList && path[i].classList.contains(selector)) {
          return true;
        }
      }
      return false;
    },
    getWeekStart() {
      moment.locale('fr');
      return moment([this.year, this.month]).weekday();
    }
  },
  data() {
    return {
      currentMonth: null,
      selectedDate: null,
      selectedDateValue: null,
      selectedYear: null,
      selectedMonth: null,
      selectedDay: null,
      year: null,
      month: null,
      months: ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Décembre'],
      amountDays: null,
      weekDays: ['Lu', 'Ma', 'Me', 'Je', 'Ve', 'Sa', 'Di']
    }
  }
}
</script>

<style scoped>
@font-face {
  font-family: "Marianne";
  src: url('../assets/fonts/Marianne-Regular.woff') format("woff2"),
  url('../assets/fonts/Marianne-Regular.woff2') format("woff");
}

.date-picker {
	position: relative;
	width: 100%;
	max-width: 315px;
	height: 40px;
	background-color: #FFF;
	margin: 30px auto;
  border-radius: 4px 4px 0 0;
	cursor: pointer;
	user-select: none;
  font-family: 'Marianne', sans-serif;
}

.date-picker:hover {
	background-color: #F3F3F3;
}

.date-picker .selected-date {
	width: 100%;
	height: 40px;

	display: flex;
	justify-content: center;
	align-items: center;

	color: #1E1E1E;
	font-size: 16px;
}

.date-picker .dates {
	display: none;
	position: absolute;
	top: 100%;
	left: 0;
	right: 0;

	background-color: #FFF;
}

.date-picker .dates.active {
	display: block;
  box-shadow: 0px 3px 10px #00000029;
  padding: 5px;
}

.date-picker .dates .month {
	display: flex;
	justify-content: space-between;
	align-items: center;
  margin-bottom: 20px;
}

.date-picker .dates .month .arrows {
	width: 35px;
	height: 35px;
	display: flex;
	justify-content: center;
	align-items: center;
	color: #313131;
	font-size: 20px;
}

.date-picker .dates .datepicker__week {
	display: grid;
	grid-template-columns: repeat(7, 1fr);
  padding: 0 14px;
}

.datepicker__day.selected {
  color: #1E1E1E;
  background-color: #9A9AFF;
  border-radius: 50%;
  font-size: 14px;
}

.datepicker__day {
  height: 31px;
  width: 41px;
  float: left;
  text-align: center;
  margin-top: 10px;
  padding-top: 10px;
}

</style>
