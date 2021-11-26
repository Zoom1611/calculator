<template>
  <div>
    <h1 class="heading">{{ heading }}</h1>
  </div>
  <CalculatorLayout />
</template>

<script>
import CalculatorLayout from "./components/CalculatorLayout.vue";

export default {
  name: "App",
  components: { CalculatorLayout },
  data() {
    return {
      heading: "Calculator",
    };
  },
};
</script>

<style>
.heading {
  color: black;
  text-align: center;
  margin-top: 25px;
  margin-bottom: 30px;
  font-size: 50px;
  background: #ed9421;
  padding: 5px 0;
}
</style>
