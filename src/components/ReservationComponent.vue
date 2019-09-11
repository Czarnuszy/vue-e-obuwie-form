<template>
  <div class="form-content">
    <div class="price-and-rating">
      <span class="price"> {{ price }} zł</span>
      <span class="rating">
        <font-awesome-icon
          icon="star"
          v-for="rate in rating"
          :key="rate"
        ></font-awesome-icon>
        {{ rating }}
      </span>
    </div>
    <div class="date-picker">
      <DatepickerComponent @getDates="getDates"></DatepickerComponent>
    </div>
    <div class="summary-and-submit">
      <button class="btn submit" type="button" @click="submit"> SUBMIT </button>
    </div>
  </div>
</template>
<script>
import DatepickerComponent from "./DatepickerComponent.vue";
export default {
  name: "ReservationComponent",
  data() {
    return {
      selectedCheckIn: null,
      selectedCheckOut: null,
    };
  },
  components: {
    DatepickerComponent
  },
  props: {
    price: {
      required: false,
      default: 100
    },
    rating: {
      required: false,
      default: 5
    }
  },
  methods: {
    submit() {
      console.log(`
        selected Check In date: ${this.selectedCheckIn.toDateString()},
        selected Check Out date: ${this.selectedCheckOut.toDateString()}
      `);
    },
    getDates(event) {
      this.selectedCheckIn = event.checkIn;
      this.selectedCheckOut = event.checkOut;
    }
  }
};
</script>
<style>
.form-content {
  padding: 1rem;
  width: 15rem;
  height: 15rem;
  margin: auto;
  background-color: var(--color1);
  border: var(--shared-border);
}

.form-content > div {
  border-bottom: var(--shared-border);
}

.price-and-rating {
  display: flex;
  flex-flow: column;
  padding-bottom: 1.5rem;
}

.price {
  font-size: 1.5rem;
  font-weight: 900;
}
.rating {
  padding-top: 0.3rem;
  font-size: 0.7rem;
  font-weight: 700;
}
.rating svg {
  color: var(--color4);
}

.date-picker {
  padding-top: .5rem;
  font-size: 0.7rem;
  font-weight: bolder;
}
.summary-and-submit {
  height: 50%;
  display: flex;
  flex-flow: column;
  justify-content: space-between;
}
.btn.submit {
  width: 100%;
  border-radius: 4px;
  border: unset;
  background: var(--color4);
  font-weight: bolder;
  color: var(--color2);
}
</style>
