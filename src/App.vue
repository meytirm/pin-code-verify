<template>
  <h1>pin code verify with Vue3</h1>
  <pin-input :digits="4" @submit-code="submitCode" />
  <count-down-timer />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import CountDownTimer from "@/components/CountDownTimer.vue";
import PinInput from "@/components/PinInput.vue";

export default defineComponent({
  name: "App",
  components: { PinInput, CountDownTimer },
  setup() {
    function submitCode(code: string) {
      alert('submit: ' + code);
    }

    return {
      submitCode
    }
  }
});
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 60px;
}
</style>
