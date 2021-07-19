<template>
<div id="app">
  <div class="abs-center calculator">
    <div class="block display-section">
      <div class="block display">
        <input  type="text" v-model="fullFormula" @keyup.enter="execute" >
        
        <div class="block small">
          {{fullFormula}}
        </div>

        <div class="block">
          {{(currentInput?currentSign:'')}}{{currentInput||result}}
        </div>
      </div>
    </div>
    <div class="block button-section">
      <div class="flex-r button-row">
        <div class="button grey" @click="clear">
          AC
        </div>

        <div class="button grey" @click="inputClear">
          C
        </div>

        <div class="button" @click="changeSign()">
          +/-
        </div>
        <div class="button" @click="addOperator('/')">/</div>
      </div>

      <div class="flex-r button-row">
        <div class="button" @click="updateNumber('7')">
          7
        </div>

        <div class="button" @click="updateNumber('8')">
          8
        </div>

        <div class="button" @click="updateNumber('9')">
          9
        </div>

        <div class="button" @click="addOperator('*')">
          *
        </div>
      </div>
      <div class="flex-r button-row">
        <div class="button" @click="updateNumber('4')">
          4
        </div>

        <div class="button" @click="updateNumber('5')">
          5
        </div>

        <div class="button" @click="updateNumber('6')">
          6
        </div>

        <div class="button" @click="addOperator('-')">
          -
        </div>
      </div>
      <div class="flex-r button-row">
        <div class="button" @click="updateNumber('1')">
          1
        </div>

        <div class="button" @click="updateNumber('2')">
          2
        </div>

        <div class="button" @click="updateNumber('3')">
          3
        </div>

        <div class="button" @click="addOperator('+')">
          +
        </div>
      </div>
      <div class="flex-r button-row">
        <div class="button wide" @click="updateNumber('0')">
          0
        </div>

        <div class="button" @click="updateNumber('.')">
          .
        </div>

        <div class="button grey" @click="execute">
          =
        </div>
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
      inputResult: ''
    };
  },
  methods: {
    updateNumber (digit) {        
      if(digit === '.') {
        if(!this.currentInput || !(this.currentInput*1)) { 
          this.currentInput = '0'
        }
        if(!(this.currentInput.includes('.'))) { 
          this.currentInput += '.'
          }
      } else {
        (!this.currentInput || (!(this.currentInput*1) && !(this.currentInput.includes('.')))) ? (
            this.currentInput = digit
        ):(
            this.currentInput += digit
        );
      }
    },
    changeSign () {
      (this.currentSign)?(this.currentSign = ''):(this.currentSign = '-')
    },
    addOperator (op) {
      if(this.result) {
        this.fullFormula = (this.result +' '+ op +' ')
        this.result = ''
      } else {
        if (this.currentInput) {
          this.fullFormula += (this.currentSign + this.currentInput +' '+ op +' ')
          this.currentSign = ''
          this.currentInput = ''
        } else {
          if(this.fullFormula.includes(' ')) {
            var temp = this.fullFormula.split('')
            temp[temp.length-2] = op
            this.fullFormula = temp.join('')
          }
        }
      }
    },
    execute () {
      if(this.fullFormula) {
        this.fullFormula += (this.currentSign + this.currentInput);
        this.result = eval(this.fullFormula);
      } else {
        this.fullFormula = this.currentSign + this.currentInput;
        this.result = this.currentInput;
      }
      this.currentSign = ''
      this.currentInput = ''
      this.fullFormula = this.result 

    },
    clear () {
      this.currentSign = ''
      this.currentInput = ''
      this.fullFormula = ''
      this.result = ''
    },
    inputClear () {
      this.currentSign = '';
      this.currentInput = '';
    },
  },
};
</script>
