<template>
  <div>
    <div class="error" v-if="error">{{ error }}</div>

    <div class="msg">
      <template v-if="result < 0">Отрицательный результат</template>
      <template v-else-if="result < 100">Результат меньше 100</template>
      <template v-else>Результат</template>
    </div>

    <div class="main">
      <input type="number" v-model.number="op1" />
      <input type="number" v-model.number="op2" />
      = {{ result }}
    </div>

    <div class="keyboard">
      <button
        v-for="operation of operations"
        @click="calculate(operation)"
        :key="operation"
      >
        {{ operation }}
      </button>
    </div>

    <!-- Чекбокс с клавиатурой -->
    <div>
      <input
        id="virtualKeyboard"
        type="checkbox"
        v-model="showKeyboard"
        :checked="showKeyboard"
      />
      <label for="virtualKeyboard">Показать клавиатуру</label>
    </div>

    <div v-show="showKeyboard">
      <div>
        <input id="operand1" type="radio" value="op1" v-model="activeOperand" />
        <label for="operand1">op 1</label>
        <input id="operand2" type="radio" value="op2" v-model="activeOperand" />
        <label for="operand2">op 2</label>
      </div>

      <div>
        <button
          v-for="digit of digits"
          :key="digit"
          @click="keyboardClick(digit)"
        >
          {{ digit }}
        </button>
        <button @click="backSpace">Backspace</button>
      </div>
    </div>
    <!-- Чекбокс с клавиатурой -->

    <br />

    <div class="logs">
      {{ logs }}
    </div>

    <button @click="counter++">{{ getCounter }}</button>
  </div>
</template>

<script>
export default {
  name: 'compCalc',
  data: () => ({
    op1: 0,
    op2: 0,
    result: 0,
    error: '',
    operations: ['+', '-', '/', '*', '^', '/='],
    logs: {},
    fibResult: 0,
    counter: 0,
    // Клавиатура
    showKeyboard: false,
    digits: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],
    activeOperand: 'op1'
  }),

  methods: {

    calculate (operation) {
      this.error = ''

      switch (operation) {
        case '+': this.sum(); break
        case '-': this.sub(); break
        case '/': this.div(); break
        case '*': this.mult(); break
        case '^': this.exp(); break
        case '/=': this.intDiv(); break
      }
      const { op1, op2, result } = this
      console.log(Date.now())
      this.$set(this.logs, Date.now(), `${op1} ${operation} ${op2} = ${result}`)
    },

    keyboardClick (digit) {
      const selectedOperandValue = this[this.activeOperand]

      if (typeof selectedOperandValue === 'number') {
        this[this.activeOperand] = Number(`${selectedOperandValue}${digit}`)
      }
    },
    backSpace () {
      const selectedOperandValue = this[this.activeOperand]

      if (typeof selectedOperandValue === 'number') {
        this[this.activeOperand] = Math.trunc(selectedOperandValue / 10)
      }
    },

    sum () {
      const { op1, op2 } = this
      this.result = op1 + op2
      this.fibResult = this.fib1 + this.fib2
    },
    div () {
      const { op1, op2 } = this

      if (op2 === 0) {
        this.error = 'На ноль делить нельзя'
        return
      }
      this.result = op1 / op2
    },
    sub () {
      const { op1, op2 } = this
      this.result = op1 - op2
    },
    mult () {
      const { op1, op2 } = this
      this.result = op1 * op2
    },
    exp () {
      const { op1, op2 } = this
      this.result = Math.pow(op1, op2)
    },
    intDiv () {
      const { op1, op2 } = this
      this.result = Math.trunc(op1 / op2)
    }
  },

  computed: {
    getCounter () {
      console.log('getCounter')
      return `Счетчик нажатий: ${this.counter}`
    }
  }
}
</script>
