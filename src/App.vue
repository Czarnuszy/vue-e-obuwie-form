<template>
  <div id="app">
    <ReservationComponent
    :price="123"
    :rating="4"></ReservationComponent>
  </div>
</template>

<script>
import ReservationComponent from "./components/ReservationComponent.vue";

export default {
  name: "app",
  components: {
    ReservationComponent
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  padding-top: 10vh;
}
:root {
  --color1: rgb(236, 236, 236);
  --color2: rgb(220, 220, 220);
  --color3: rgb(164, 194, 168);
  --color4: rgb(81, 163, 163);
  --color5: rgba(81, 163, 163, 0.76);
  --shared-border: 1px var(--color2) solid;
}
</style>
