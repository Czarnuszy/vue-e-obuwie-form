<template>
  <div class="date-picker" v-outside="handleOutside">
    <span class="datepicker-caption">Dates</span>
    <div class="date-picker__input-wrapper">
      <input
        v-model="getCheckInDate"
        :class="{ focused: isCheckInFocused }"
        placeholder="Check In"
        type="text"
        @click="isEdit = true"
      />
      <font-awesome-icon icon="arrow-right" />
      <input
        v-model="getCheckOutDate"
        @click="isEdit = true; isCheckOutFocused = true"
        :class="{ focused: isCheckOutFocused }"
        placeholder="Check Out"
        ref="checkOutInput"
        type="text"
      />
    </div>
    <div class="date-picker__calendar--wrapper">
      <CalendarComponent
        v-show="isEdit"
        :excluded-dates="excludedDates"
        :selected-check-in="checkInDate"
        :selected-check-out="checkOutDate"
        @checkIn="setCheckIn"
        @checkOut="setCheckOut"
      ></CalendarComponent>
    </div>
  </div>
</template>
<script>
import CalendarComponent from "./CalendarComponent.vue";
import Outside from "../utils/clickoutside.js";

export default {
  name: "DatepickerComponent",
  directives: { Outside },
  components: {
    CalendarComponent
  },
  data() {
    return {
      isEdit: false,
      isCheckOutFocused: false,
      isCheckInFocused: false,
      checkInDate: null,
      checkOutDate: null,
      excludedDates: [new Date(2019, 10, 28).toLocaleDateString()]
    };
  },
  computed: {
    getCheckInDate() {
      if (this.checkInDate) {
        return this.checkInDate.toLocaleDateString();
      }
      return "";
    },
    getCheckOutDate() {
      if (this.checkOutDate) {
        return this.checkOutDate.toLocaleDateString();
      }
      return "";
    }
  },
  methods: {
    setCheckIn(event) {
      this.checkInDate = event;
    },
    setCheckOut(event) {
      this.checkOutDate = event;
    },
    handleOutside() {
      this.isEdit = false;
      this.$emit("getDates", {checkIn: this.checkInDate, checkOut: this.checkOutDate})
    }
  }
};
</script>
<style>
.date-picker__input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  flex-flow: row;
  justify-content: space-between;
  border: var(--shared-border);
  padding: 0.2rem;
  color: var(--color4);
}
.date-picker__input-wrapper input {
  width: 40%;
  border: none;
  background-color: var(--color1);
  outline: none;
  border-radius: 4px;
}
.date-picker__input-wrapper input.focused {
  background-color: var(--color4);
}

.date-picker__calendar--wrapper {
  position: relative;
  width: 20vw;
}
</style>
