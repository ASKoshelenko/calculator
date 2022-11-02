<template>
  <h1>Easy Calc</h1>
  <div class="calculator">
    <div @keyup="equal" class="display">{{current || '0'}}</div>
    <div @click="undo" :disabled="!canUndo" class="btn undo">⟲</div>
    <div @click="redo" :disabled="!canRedo" class="btn redo">⟳</div>
    <div @click="clear" class="btn">AC</div>
    <div @click="sign" class="btn">+/-</div>
    <div @click="percent" class="btn">%</div>
    <div @click="divide" class="btn operator">÷</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="times" class="btn operator">x</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="minus" class="btn operator">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="plus" class="btn operator">+</div>
    <div @click="append('0')" class="btn double-column">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="calculate" class="btn operator">=</div>
  </div>
</template>

<script>
const computedDataHistory = []
const history = []
const historyIndex = history.length - 1;
export default {
  name: 'easy-calc',
  data() {
    return {
      previous: null,
      current: '',
      operator: null,
      operatorClicked: false,
      //
      newHistory: undefined,
      computedData: computedDataHistory,
      history: history,
      historyIndex: historyIndex,
      edited: false
    }
  },
  computed: {
    canUndo: function() {
      return this.historyIndex > 0
    },
    canRedo: function() {
      return this.history.length - 1 - this.historyIndex > 0
    }
  },
  methods: {
    //
    add() {
      if (this.history) {
        this.computedData.push()
        this.newHistory = undefined
      }
      this.history.push()
    },
    undo() {
      console.log(history)
      console.log(`current ${this.current}`)
      console.log(`previous ${this.history[1]}`)
      // this.current = this.history[1]
      // console.log(`new current ${this.current}`)
      

      if (this.canUndo) {
        this.historyIndex -= 1
        this.current = this.history[this.historyIndex]
      }
    },
    redo() {
      console.log(history)
      console.log(`current ${this.current}`)
      console.log(`next ${this.history[history.length - 1]}`)
      if (this.canRedo) {
        this.historyIndex += 1
        this.current = this.history[this.historyIndex]
      }
    },

    //
    clear() {
      this.current = '';
    },
    sign() {
      this.current = this.current.charAt(0) === '-' ? 
        this.current.slice(1) : `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    times() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    minus() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    plus() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    calculate() {
      console.log(this.current)
      this.current = `${this.operator(
        parseFloat(this.previous),
        parseFloat(this.current)        
        )}`;
        this.previous = null;
        this.add(this.current);
        console.log(this.current)
        history.push(this.current)
        console.log(history)
      }
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
</style>
