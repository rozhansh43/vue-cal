<template>
<div id="app">
  <div class="abs-center calculator">
    <div class="rela-block display-section">
      <div class="rela-block display">
        <div class="rela-block small">{{fullFormula}}</div>
        <div class="rela-block">{{(currentInput?currentSign:'')}}{{currentInput||result}}</div>
      </div>
    </div>
    <div class="rela-block button-section">
        <div class="flex-r button-row">
          <div class="flex button blue" @click="clear">AC</div>
          <div class="flex button blue" @click="inputClear">C</div>
          <div class="flex button" @click="changeSign()">+/-</div>
          <div class="flex button" @click="addOperator('/')">/</div>
        </div>
        <div class="flex-r button-row">
          <div class="flex button" @click="updateNumber('7');">7</div>
          <div class="flex button" @click="updateNumber('8');">8</div>
          <div class="flex button" @click="updateNumber('9');">9</div>
          <div class="flex button" @click="addOperator('*')">*</div>
        </div>
        <div class="flex-r button-row">
          <div class="flex button" @click="updateNumber('4');">4</div>
          <div class="flex button" @click="updateNumber('5');">5</div>
          <div class="flex button" @click="updateNumber('6');">6</div>
          <div class="flex button" @click="addOperator('-')">-</div>
        </div>
        <div class="flex-r button-row">
          <div class="flex button" @click="updateNumber('1');">1</div>
          <div class="flex button" @click="updateNumber('2');">2</div>
          <div class="flex button" @click="updateNumber('3');">3</div>
          <div class="flex button" @click="addOperator('+')">+</div>
        </div>
      <div class="flex-r button-row">
        <div class="flex button wide" @click="updateNumber('0');">0</div>
        <div class="flex button" @click="updateNumber('.');">.</div>
        <div class="flex button blue" @click="equal">=</div>
      </div>
    </div>
  </div>
</div>

</template>

<script>
export default {
  data() {
    return {
      currentSign: '',
      currentInput: '',
      fullFormula: '',
      result: '',
    };
  },
  methods: {
        updateNumber (digit) {
            // if(this.result) { app.clear(); }
            
            if(digit === '.') {
                if(!this.currentInput || !(this.currentInput*1)) { this.currentInput = '0'; }
                if(!(this.currentInput.includes('.'))) { this.currentInput += '.'; }
            } else {
                (!this.currentInput || (!(this.currentInput*1) && !(this.currentInput.includes('.')))) ? (
                    this.currentInput = digit
                ):(
                    this.currentInput += digit
                );
            }
        },
        changeSign () {
            (this.currentSign)?(this.currentSign = ''):(this.currentSign = '-');
        },
        addOperator (op) {
            if(this.result) {
                this.fullFormula = (this.result+' '+op+' ');
                this.result = '';
            } else {
                if(this.currentInput) {
                    this.fullFormula += (this.currentSign+this.currentInput+' '+op+' ');
                    this.currentSign = '';
                    this.currentInput = '';
                } else {
                    if(this.fullFormula.includes(' ')) {
                        var temp = this.fullFormula.split('');
                        temp[temp.length-2] = op;
                        this.fullFormula = temp.join('');
                    }
                }
            }
        },
        execute () {
            if(this.fullFormula) {
                this.fullFormula += (this.currentSign+this.currentInput);
                this.result = eval(this.fullFormula);
            } else {
                this.fullFormula = this.currentSign+this.currentInput;
                this.result = this.currentInput;
            }
            this.currentSign = '';
            this.currentInput = '';
        },
        clear () {
            this.currentSign = '';
            this.currentInput = '';
            this.fullFormula = '';
            this.result = '';
        },
        inputClear () {
            this.currentSign = '';
            this.currentInput = '';
        },
        equal () {
          if(this.currentInput){this.execute()}
        }
    },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
  transition: 0.25s ease;
}
.abs-center {
  position: absolute;
  top: 50%;
  left: 50%;
  right: 0;
  bottom: 0;
  transform: translate(-50%, -50%);
}
.rela-block {
  display: block;
  position: relative;
}
.rela-inline {
  display: inline-block;
  position: relative;
}
.flex-r {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
}
.flex-c {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
}
.flex {
  flex: 1 1 0;
}
body {
  transition: 0s;
  font-family: monospace;
  font-size: 18px;
  background-color: #111;
  color: #eee;
}
.small {
  font-size: 14px;
  margin-bottom: 10px;
}
.calculator {
  width: 420px;
  padding: 10px;
}
.display {
  margin: 10px 5px;
  background-color: #000;
  font-size: 36px;
  letter-spacing: 4px;
  text-align: right;
  padding: 15px 10px;
  overflow: hidden;
  border: 1px solid #444;
  border-radius: 3px;
}
.display *::before {
  content: ".";
  color: rgba(0,0,0,0);
}
.button {
  margin: 5px;
  text-align: center;
  padding: 20px 5px;
  max-width: 90px;
  background-color: rgba(255,255,255,0.1);
  border-radius: 3px;
}
.button:hover,
.button:active {
  background-color: rgba(255,255,255,0.3);
  cursor: pointer;
}
.button.wide {
  flex: 2 1 0;
  max-width: 500px;
}
.button.wider {
  flex: 3 1 0;
  max-width: 500px;
}
.button.blue {
  background-color: rgba(0,50,215,0.5);
}
.button.blue:hover,
.button.blue:active {
  background-color: #0032d7;
}
.button.hidden {
  cursor: default;
  opacity: 0;
}
.button.disabled {
  cursor: default;
  color: rgba(255,255,255,0.2);
}
.button.disabled:hover,
.button.disabled:active {
  background-color: rgba(255,255,255,0.1);
}

</style>