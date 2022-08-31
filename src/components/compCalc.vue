<template>
  <div>
    <div class="error" v-if="error">{{ error }}</div>
    <!--    <div class="error" v-show="error">{{ error }}</div>-->
    <div class="msg">
      <template v-if="result < 0">Отрицательный результат</template>
      <template v-else-if="result < 100">Результат меньше 100</template>
      <template v-else>Результат</template>
    </div>
    <div class="main">
      <input type="number" v-model.number="op1" />
      <input type="number" v-model.number="op2" />
      = {{ result }}<br />
      fib = {{ fibResult }}
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
    operations: ['+', '-', '/', '*'],
    logs: {},
    fibResult: 0,
    counter: 0
  }),
  methods: {

    calculate (operation) {
      this.error = ''

      switch (operation) {
        case '+': this.sum(); break
        case '-': this.sub(); break
        case '/': this.div(); break
        case '*': this.mult(); break
      }

      const { op1, op2, result } = this
      console.log(Date.now())
      this.$set(this.logs, Date.now(), `${op1} ${operation} ${op2} = ${result}`)
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
    fib1 () {
      return this.fib(this.op1)
    },
    fib2 () {
      return this.fib(this.op2)
    },
    getCounter () {
      console.log('getCounter')
      return `Счетчик нажатий: ${this.counter}`
    }
  }
}
</script>
