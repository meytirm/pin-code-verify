<template>
  <div>
    <div v-if="timer">{{ timer }} to resend the code</div>
    <div>
      <button v-if="!timer && counter < 0" @click="setTimer()">
        reset
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "CountDownTimer",
  props: {
    seconds: {
      default: 5,
      type: Number,
    },
  },
  data() {
    return {
      counter: 5,
      timer: "",
      interval: null,
    };
  },
  mounted() {
    this.setTimer()
  },
  methods: {
    setTimer() {
      this.counter = this.seconds;
      this.interval = setInterval(() => {
        this.countDown();
      }, 1000);
    },
    countDown() {
      if (this.counter >= 0) {
        this.timer = this.formatSeconds(this.counter);
        this.counter--;
      } else {
        this.expired = true;
        clearInterval(this.interval);
        this.interval = null;
        this.timer = "";
      }
    },

    formatSeconds(secs) {
      function pad(n) {
        return n < 10 ? "0" + n : n;
      }

      const h = Math.floor(secs / 3600);
      const m = Math.floor(secs / 60) - h * 60;
      const s = Math.floor(secs - h * 3600 - m * 60);

      return pad(m) + ":" + pad(s);
    },
  },
};
</script>

<style lang="scss" scoped></style>
