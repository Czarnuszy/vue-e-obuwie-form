<template>
  <div class="calendar">
    <transition name="transition-drop">
      <div class="date-picker__calendar">
        <div class="calendar__header">
          <font-awesome-icon
            icon="angle-left"
            @click="prevMonth"
          ></font-awesome-icon>
          <h3>
            {{ month.toLocaleString("default", { month: "long" }) }}
            {{ month.getFullYear() }}
          </h3>
          <font-awesome-icon
            icon="angle-right"
            @click="nextMonth"
          ></font-awesome-icon>
        </div>
        <div class="calendar__table">
          <table>
            <thead>
              <tr class="table__week">
                <td :key="day" v-for="day of weekDays">{{ day }}</td>
              </tr>
            </thead>
            <tbody>
              <tr v-for="row of rows" :key="rows.indexOf(row)">
                <td
                  :key="date.toString()"
                  @click="pickDate(date)"
                  class="table__day"
                  v-for="date in row"
                >
                  <div
                    :class="{
                      'edge-start': selectedCheckIn === date,
                      'edge-end': selectedCheckOut === date,
                      'in-selected-range': isInSelectedRange(date),
                      'current-date': isToday(date),
                      disabled: date < currentDate || date < selectedCheckIn || isDateDisabled(date),
                      'prev-month': date.getMonth() !== curMonth
                    }"
                    :disabled="date < selectedCheckIn || isDateDisabled(date)"
                  >
                    {{ date.getDate() }}
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import Outside from "../utils/clickoutside.js";

export default {
  name: "CalendarComponent",
  directives: { Outside },
  data() {
    return {
      weekDays: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
      month: this.selectedCheckIn || new Date(),
      currentDate: new Date(),
    };
  },
  props: {
    selectedCheckIn: {
      required: false
    },
    selectedCheckOut: {
      required: false,
    },
    excludedDates: {
      required: false,
      default: []
    }
  },
  computed: {
    days() {
      let daysArr = [];
      let firstDayOfMonth = new Date(this.curYear, this.curMonth, 1);
      let lastDayOfMonth = new Date(
        this.curYear,
        this.curMonth,
        this.getLastDayOfMonth(this.curMonth)
      );
      daysArr.unshift(firstDayOfMonth);
      for (
        let d = this.prevDate(firstDayOfMonth);
        d.getDay() !== 0;
        d = this.prevDate(d)
      ) {
        daysArr.unshift(d);
      }
      for (
        let d = this.nextDate(firstDayOfMonth);
        d <= lastDayOfMonth;
        d = this.nextDate(d)
      ) {
        daysArr.push(d);
      }
      for (
        let d = this.nextDate(lastDayOfMonth);
        d.getDay() !== 1;
        d = this.nextDate(d)
      ) {
        daysArr.push(d);
      }
      return daysArr;
    },
    rows() {
      return this.days.reduce((prevDays, curDay, index) => {
        if (index % 7 === 0) {
          prevDays[prevDays.length] = [];
        }
        prevDays[prevDays.length - 1].push(curDay);
        return prevDays;
      }, []);
    },
    curDay() {
      return this.month.getDate();
    },
    curMonth() {
      return this.month.getMonth();
    },
    curYear() {
      return this.month.getFullYear();
    }
  },
  methods: {
    isDateDisabled(date) {
      return this.excludedDates.includes(date.toLocaleDateString());
    },
    isToday(date) {
      return (
        this.currentDate.toLocaleDateString() === date.toLocaleDateString()
      );
    },
    isInSelectedRange(date) {
      return this.selectedCheckIn < date && this.selectedCheckOut > date;
    },
    nextMonth() {
      this.month = new Date(this.month.setMonth(this.month.getMonth() + 1));
    },
    prevMonth() {
      this.month = new Date(this.month.setMonth(this.month.getMonth() - 1));
    },
    getLastDayOfMonth(month) {
      let lastDaysArr = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
      return lastDaysArr[month];
    },
    nextDate(date) {
      let ret = new Date(date);
      ret.setDate(date.getDate() + 1);
      return ret;
    },
    prevDate(date) {
      let ret = new Date(date);
      ret.setDate(date.getDate() - 1);
      return ret;
    },
    pickDate(date) {
      if (!this.selectedCheckIn) {
        this.$emit("checkIn", date);
      } else {
        this.$emit("checkOut", date);
      }
    }
  }
};
</script>
<style>
table {
  border-collapse: collapse;
  border-spacing: unset;
}

.date-picker__calendar {
  transform-origin: top;
  transition: transform 0.4s ease-in-out;
  position: absolute;
  margin-top: 5px;
  box-shadow: 0 0.5px #ffffff inset, 0 1px 2px 0 #b3b3b3;
  background: #ffffff;
  width: 16rem;
}

.calendar__header {
  color: #fff;
  padding: 0 10px;
  font-size: 1rem;
  background: var(--color4);
  display: flex;
  flex-flow: row;
  justify-content: space-between;
  align-items: center;
}

.calendar__header svg {
  color: #000;
  cursor: pointer;
}

.calendar__table {
  display: flex;
  padding: 0.5rem 0;
  justify-content: center;
}

td {
  font-size: 0.9rem;
  font-weight: normal;
}

td.table__day div {
  cursor: pointer;
  width: 2rem;
  height: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

thead td {
  color: var(--color2);
}

td .prev-month {
  color: var(--color2);
}
td .current-day {
  border: 2px solid var(--color4);
  border-radius: 50%;
}
td .disabled {
  color: var(--color2);
  cursor: not-allowed;
}

.edge-end,
.edge-start {
  background: var(--color4);
  color: #fff;
  border-radius: 50%;
}
.edge-start {
  border-top-right-radius: unset;
  border-bottom-right-radius: unset;
}
.edge-end {
  border-top-left-radius: unset;
  border-bottom-left-radius: unset;
}
.in-selected-range {
  background: var(--color5);
  color: #fff;
}
</style>
