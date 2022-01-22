<template>
  <div class="wrapper">
    <input
      class="pin-input col-2 border-radius-8 q-py-xs bg-tes-grey-light"
      v-for="(digit, index) in digits"
      :key="digit"
      type="text"
      @input="movement"
      @keydown="moveBack"
      maxlength="1"
      :ref="setInputRef"
      :data-index="index"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'

export default defineComponent(
  {
    name: "PinInput",
    emits: ["submitCode"],
    props: {
      digits: {
        default: 4,
        type: Number,
      },
    },
    setup(props, { emit }) {
      const inputRefs = ref<HTMLInputElement[]>([])
      function setInputRef(el: HTMLInputElement) {
        if (el && inputRefs.value.length < props.digits) {
          inputRefs.value.push(el)
        }
      }

      const moveBack = function (event: KeyboardEvent) {
        const target = event.target as HTMLInputElement
        const inputIndex = target.getAttribute('data-index')
        if (event.key === 'Backspace' && Number(inputIndex) > 0) {
          setTimeout(() => {
            const previousInput = inputRefs.value[Number(inputIndex) - 1]
            previousInput.focus()
            previousInput.select()
          }, 100)
        }
      }

      const movement = function (event: InputEvent) {
        const value = event.data
        const target = event.target as HTMLInputElement
        if (value) {
          if (isNumber(value)) {
            const inputIndex = target.getAttribute('data-index')
            if (inputIndex) {
              if (allowBackspace(event, inputIndex)) {
                const nextInput = inputRefs.value[Number(inputIndex) + 1]
                nextInput.focus()
              }
            }
            const code = inputRefs.value.map((input) => input.value).join('')
            // emit('update:pin-code', code)
            if (code.length === props.digits) {
              sendCode(code)
            }
          } else {
            event.preventDefault()
            target.value = ''
          }
        }
      }

      function isNumber(value: string) {
        return Number.isInteger(parseInt(value))
      }

      function allowBackspace(event: InputEvent, inputIndex: string) {
        return (
          event.inputType !== 'deleteContentBackward' && Number(inputIndex) < 3
        )
      }

      function sendCode(code: string) {
        emit('submitCode', code)
      }

      onMounted(() => {
        inputRefs.value[0].focus()
      })

      return {
        movement,
        moveBack,
        setInputRef,
      }
    },
  }

)
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
