<template>
  <h1>Easy Calc</h1>
  <div class="calculator">
    <div @keyup="equal" class="display">{{calculate(current) || '0'}}</div>
    <div @click="undo" class="btn undo">⟲</div>
    <div @click="redo" class="btn redo">⟳</div>
    <div @click="clear" class="btn reset">Clear</div>
    <div @click="append('/')" class="btn operator">÷</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="append('*')" class="btn operator">x</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="append('-')" class="btn operator">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="append('+')" class="btn operator">+</div>
    <div @click="append('0')" class="btn double-column">0</div>
    <div @click="append('.')" class="btn">.</div>
    <div @click="append('=')" class="btn operator">=</div>

  </div>
</template>

<script>
export default {
  name: 'easy-calc',
  data() {
    return {
      current: '',
    }
  },
  computed: {},
  methods: {
    calculate(str) {
      const chars = [
        '0', '1', '2', '3', '4', 
        '5', '6', '7', '8', '9',
        '.', '=', '+', '-', '*', '/',
      ]

      if (!str.split('').every(char => chars.includes(char))) {
        return 'NaN'
      }
      const expressions = str.split('=')
      const last = expressions.length - 1

      if (!last || !str) {
        return str
      }

      return `${
        expressions
          .filter((_, idx) => idx !== last)
          .reduce((acc, expression) => eval(`${acc}${expression}`), '0+')
      }${expressions[last]}`
    },
    undo() {
      if (this.current) {
        this.history.push(this.current.slice(-1))
        this.current = this.current.slice(0,-1)
      }
    },
    redo() {
      if (this.history.length) {
        this.current = this.current + this.history.pop()
      }
    },
    clear() {
      this.current = ''
      this.history = []
    },
    append(char) {
      if (char == '=') {
        this.history = []
      }
      this.current = `${this.current}${char}`
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .calculator {
    width: 300px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
    font-size: 40px;
    /* box-shadow: 25px 15px 10px 5px rgba(10, 10, 10, 0.2); */
  }

  .double-column {
    grid-column: 1 / 3;
    background-color: bisque;
  }

  .btn {
    background-color: #f2f2f2;
    border: 1px solid #999;
    border-radius: 10px;
    cursor: pointer;
    box-shadow: 1px 2px 1px 1px rgba(34, 71, 80, 0.2);

  }
  
  .btn:hover {
    transform: translate(0em, 0.01em);
    opacity: 80%;
    /* background-color: transparent; */
    /* color: black; */
  }

  .btn:active {
    background: rgb(236, 234, 200);
    transform: translate(0em, 0.02em);
  }

  .operator {
    background-color: orange;
    color: white;
  }

  .display {
    grid-column: 1 / 5;
    background-color: #333;
    color: white;
    border: 1px solid #999;
    border-radius: 10px;
  }

  .undo {
    background-color: rgb(0, 221, 250);
    grid-column: 1/3;
  }

  .redo {
    background-color: rgb(215, 235, 36);
    grid-column: 3/5;
  }

  .reset {
    grid-column: 1/4;
    background-color: rgb(243, 131, 87);
  }
</style>
