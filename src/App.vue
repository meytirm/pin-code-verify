<template>
  <h1>pin code verify with Vue3</h1>
  <pin-input :digits="4" @submit-code="submitCode" />
  <count-down-timer />
</template>

<script>
import PinInput from "@/components/PinInput";
import CountDownTimer from "./components/CountDownTimer.vue";

export default {
  name: "App",
  components: {
    PinInput,
    CountDownTimer,
  },
  methods: {
    submitCode(code) {
      alert("submit: " + code);
    },
  },
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 60px;
}
</style>
