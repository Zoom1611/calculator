<template>
  <div class="center-items">
    <div class="calculator col-lg-4">
      <div class="row col-lg-12 result">{{ printSelectedNumber || 0 }}</div>
      <div class="row col-lg-12 numbers-symbols">
        {{ printSelectedNumberAndSymbol }}
      </div>
      <div class="row center-items">
        <div class="col-lg-2 box-style3 center-items" @click="clearAll()">
          AC
        </div>
        <div class="col-lg-2 box-style3 center-items" @click="clearOne()">
          C
        </div>
        <div class="col-lg-2 box-style3 center-items"></div>
        <div
          class="col-lg-2 box-style2 center-items"
          @click="symbolSelection('/')"
        >
          /
        </div>
      </div>
      <div class="row center-items">
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('7')"
        >
          7
        </div>
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('8')"
        >
          8
        </div>
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('9')"
        >
          9
        </div>
        <div
          class="col-lg-2 box-style2 center-items"
          @click="symbolSelection('*')"
        >
          *
        </div>
      </div>
      <div class="row center-items">
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('4')"
        >
          4
        </div>
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('5')"
        >
          5
        </div>
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('6')"
        >
          6
        </div>
        <div
          class="col-lg-2 box-style2 center-items"
          @click="symbolSelection('-')"
        >
          -
        </div>
      </div>
      <div class="row center-items">
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('1')"
        >
          1
        </div>
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('2')"
        >
          2
        </div>
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('3')"
        >
          3
        </div>
        <div
          class="col-lg-2 box-style2 center-items"
          @click="symbolSelection('+')"
        >
          +
        </div>
      </div>
      <div class="row center-items">
        <div
          class="col-lg-4 box-style1 center-items zero-style"
          @click="numberSelection(0)"
        >
          0
        </div>
        <div
          class="col-lg-2 box-style1 center-items"
          @click="numberSelection('.')"
        >
          .
        </div>
        <div class="col-lg-2 box-style2 center-items" @click="equal">=</div>
      </div>
    </div>
  </div>
  <div class="numbers-simbols"></div>
</template>

<script>
export default {
  data() {
    return {
      selectedNumbers: [],
      selectedSymbols: [],
      printSelectedNumber: "",
      printSelectedNumberAndSymbol: "",
      number: "",
      firstNumber: null,
      secondNumber: null,
      result: null,
    };
  },
  methods: {
    numberSelection(selected) {
      this.number += selected;
      this.printSelectedNumber = this.number;
    },
    symbolSelection(selected) {
      this.selectedSymbols.push(selected);
      this.printSelectedNumberAndSymbol += this.number + selected;
      if (this.number !== "") {
        this.selectedNumbers.push(parseFloat(this.number));
      }
      if (this.selectedSymbols.length > this.selectedNumbers.length) {
        this.selectedSymbols.splice(-2);
        this.selectedSymbols.push(selected);
        this.printSelectedNumberAndSymbol =
          this.printSelectedNumberAndSymbol.slice(0, -2);
        this.printSelectedNumberAndSymbol += selected;
      }
      if (this.printSelectedNumberAndSymbol.indexOf("=", 0) !== -1) {
        this.printSelectedNumberAndSymbol =
          this.selectedNumbers[0] + this.selectedSymbols[0];
      }
      this.number = "";
      console.log(this.selectedNumbers);
      console.log(this.selectedSymbols);
      console.log(this.printSelectedNumberAndSymbol);
    },
    equal() {
      parseFloat(this.number);
      this.selectedNumbers.push(parseFloat(this.number));
      this.printSelectedNumberAndSymbol += this.number += "=";
      this.number = "";
      if (this.selectedNumbers.length < 3) {
        debugger;
        this.firstNumber = this.selectedNumbers[0];
        this.secondNumber = this.selectedNumbers[1];
        let symbol = this.selectedSymbols[0];
        this.selectedNumbers = [];
        this.selectedSymbols = [];
        this.selectedNumbers.unshift(
          this.test(this.firstNumber, this.secondNumber, symbol)
        );
      } else if (this.selectedNumbers.length > 2) {
        for (let index = 0; index < this.selectedSymbols.length; index++) {
          if (
            this.selectedSymbols[index] === "*" ||
            this.selectedSymbols[index] === "/"
          ) {
            this.firstNumber = this.selectedNumbers[index];
            this.secondNumber = this.selectedNumbers[index + 1];
            if (
              !this.selectedNumbers[index] ||
              !this.selectedNumbers[index + 1]
            ) {
              this.firstNumber =
                this.selectedNumbers[this.selectedNumbers.length - 2];
              this.secondNumber =
                this.selectedNumbers[this.selectedNumbers.length - 1];
              this.result = this.test(
                this.firstNumber,
                this.secondNumber,
                this.selectedSymbols[index]
              );
              this.selectedNumbers.splice(
                this.selectedNumbers.length - 2,
                2,
                this.result
              );
            } else {
              this.result = this.test(
                this.firstNumber,
                this.secondNumber,
                this.selectedSymbols[index]
              );
              this.selectedNumbers.splice(index, 2, this.result);
            }
            if (this.selectedNumbers.length === 1) {
              this.printSelectedNumber = this.selectedNumbers[0];
              this.selectedSymbols.splice(0, index + 1);
            }
          }
        }

        for (let i = 0; i < this.selectedSymbols.length; i++) {
          if (
            this.selectedSymbols[i] === "+" ||
            this.selectedSymbols[i] === "-"
          ) {
            this.firstNumber = this.selectedNumbers[0];
            this.secondNumber = this.selectedNumbers[1];
            this.selectedNumbers.splice(0, 2);
            this.selectedNumbers.unshift(
              this.test(
                this.firstNumber,
                this.secondNumber,
                this.selectedSymbols[i]
              )
            );
            if (this.selectedNumbers.length === 1) {
              this.printSelectedNumber = this.selectedNumbers[0];
              this.selectedSymbols.splice(0, i + 1);
            }
          }
        }
      }
    },
    clearAll() {
      this.selectedNumbers = [];
      this.selectedSymbols = [];
      this.printSelectedNumber = "";
      this.printSelectedNumberAndSymbol = "";
      this.number = "";
      this.firstNumber = null;
      this.secondNumber = null;
      this.result = null;
    },
    clearOne() {
      this.printSelectedNumber = "";
      this.number = "";
    },
    test(x, y, sign) {
      switch (sign) {
        case "+":
          return (this.printSelectedNumber = x + y);
        case "-":
          return (this.printSelectedNumber = x - y);
        case "*":
          return (this.printSelectedNumber = x * y);
        case "/":
          return (this.printSelectedNumber = x / y);
      }
      return 0;
    },
  },
};
</script>

<style>
.calculator {
  height: 500px;
  color: white;
  font-size: 30px;
  display: flex;
  justify-content: center;
  flex-direction: column;
}
.center-items {
  display: flex;
  justify-content: center;
  align-items: center;
}
.box-style1 {
  background: #333333;
  border: none;
  border-radius: 50%;
  height: 80px;
  cursor: pointer;
}
.box-style2 {
  background: #ed9421;
  border: none;
  border-radius: 50%;
  height: 80px;
  cursor: pointer;
}
.box-style3 {
  background: #a5a5a5;
  color: black;
  border: none;
  border-radius: 50%;
  height: 80px;
  cursor: pointer;
}
div .col-lg-3 {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 10px;
}
.result {
  font-size: 50px;
  justify-content: flex-end;
  max-width: 430px;
}
.zero-style {
  border-radius: 50px;
}
.numbers-symbols {
  font-size: 24px;
  justify-content: flex-end;
  max-width: 430px;
  margin: 10px;
  padding: 10px;
}
</style>
