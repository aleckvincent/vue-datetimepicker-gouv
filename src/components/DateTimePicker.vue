<template>
  <div class="date-picker" @click="toggleDatePicker($event)">
        <div class="selected-date" :data-value="selectedDateValue">{{ selectedDate }}</div>

        <div class="dates" ref="dates">
            <div class="month">
                <div class="arrows prev-mth" @click="goToPrevMonth">&lt;</div>
                <div class="mth">{{ currentMonth }}</div>
                <div class="arrows next-mth" @click="goToNextMonth">&gt;</div>
            </div>

            <div class="days">
              <div
                   v-for="index in amountDays" :key="index"
                   class="day"
                   v-bind:class="selectedDay === (index+1) && selectedYear === year && selectedMonth === month ? 'selected' : ''"
                   @click="onSelectDate(index)"
              >

                {{ index }}
              </div>
            </div>
        </div>
    </div>
</template>

<script>
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
      this.selectedDay = day;
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
      months: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
      amountDays: null
    }
  }
}
</script>

<style scoped>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}

body {
	background-color: #FFCE00;
	font-family: 'Saira', Arial, Helvetica, sans-serif;
}

h1 {
	margin: 30px 0px;
	color: #313131;
	font-size: 42px;
	font-weight: 900;
	text-align: center;
}

h1 span {
	font-weight: 300;
}

.date-picker {
	position: relative;
	width: 100%;
	max-width: 320px;
	height: 60px;
	background-color: #FFF;
	margin: 30px auto;
	box-shadow: 0px 3px 10px rgba(0, 0, 0, 0.2);

	cursor: pointer;
	user-select: none;
}

.date-picker:hover {
	background-color: #F3F3F3;
}

.date-picker .selected-date {
	width: 100%;
	height: 100%;

	display: flex;
	justify-content: center;
	align-items: center;

	color: #313131;
	font-size: 28px;
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
}

.date-picker .dates .month {
	display: flex;
	justify-content: space-between;
	align-items: center;
	border-bottom: 2px solid #EEE;
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

.date-picker .dates .month .arrows:hover {
	background-color: #F3F3F3;
}

.date-picker .dates .month .arrows:active {
	background-color: #00CA85;
}

.date-picker .dates .days {
	display: grid;
	grid-template-columns: repeat(7, 1fr);
	height: 200px;
}
.date-picker .dates .days .day { 
	display: flex;
	justify-content: center;
	align-items: center;
	color: #313131;
}
.date-picker .dates .days .day.selected {
	background-color: #00CA85;
}

</style>
