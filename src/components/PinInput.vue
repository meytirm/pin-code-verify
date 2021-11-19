<template>
  <div class="wrapper">
    <input
      class="pin-input col-2 border-radius-8 q-py-xs bg-tes-grey-light"
      @input="inputMovement"
      v-for="(digit, index) in digits"
      pattern="\d*"
      maxlength="1"
      :data-index="index"
      :key="digit"
      :ref="setInputRef"
    />
  </div>
</template>

<script>
export default {
  name: "PinInput",
  data() {
    return {
      inputRefs: [],
    };
  },
  emits: ["submitCode"],
  props: {
    digits: {
      default: 4,
      type: Number,
    },
  },
  methods: {
    setInputRef(el) {
      if (el) {
        this.inputRefs.push(el);
      }
    },
    inputMovement(e) {
      const currentInputIndex = Number(e.target.attributes["data-index"].value);
      const typeOfInput = e.inputType;
      const allInputs = this.inputRefs;
      const stepMoveInput = 1;
      const code = this.inputRefs.map((input) => input.value).join("");

      // enter number
      if (typeOfInput === "insertText") {
        // check if next input available
        if (allInputs[currentInputIndex + stepMoveInput]) {
          allInputs[currentInputIndex + stepMoveInput].focus();
          if (code.length === this.digits) {
            this.submitCode(code);
          }
        } else {
          if (code.length === this.digits) {
            this.submitCode(code);
          }
        }
      }

      // backspace
      if (typeOfInput === "deleteContentBackward") {
        if (allInputs[currentInputIndex - stepMoveInput]) {
          allInputs[currentInputIndex - stepMoveInput].focus();
        }
      }
    },
    submitCode(code) {
      this.$emit("submitCode", code);
    },
  },
};
</script>

<style lang="css" scoped>
.wrapper {
  width: 50%;
  display: flex;
  justify-content: space-between;
}

.pin-input {
  width: 10%;
  text-align: center;
  border-radius: 8px;
  border: 2px solid grey;
  background: #f4f4f4;
  padding: 10px;
  font-size: 18px;
  font-weight: bold;
}

.pin-input:focus {
  outline: none;
  border: 2px solid deepskyblue;
}
</style>
