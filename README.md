# weather-app

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

//

### App ID

OMq5lyFk

### Client ID (Consumer Key)

dj0yJmk9WHVUVEkxZVhNcGFzJmQ9WVdrOVQwMXhOV3g1Um1zbWNHbzlNQT09JnM9Y29uc3VtZXJzZWNyZXQmc3Y9MCZ4PWVk

### Client Secret (Consumer Secret)

aea9d82b304be124ab6bfb33eb53328b28755954

## Just in case

<template>
  <div class="row center-items">
    <div class="calculator col-lg-4">
      <div class="result">{{ printSelectedNumber || 0 }}</div>
      <div class="numbers-symbols">
        {{ printSelectedNumber }} {{ printSelectedSymbol }}
      </div>
      <div class="row center-items">
        <div class="col-lg-3 box-style3" @click="clearAll('C')">C</div>
        <div class="col-lg-3 box-style3"></div>
        <div class="col-lg-3 box-style3"></div>
        <div class="col-lg-3 box-style2" @click="symbolSelection('/')">/</div>
      </div>
      <div class="row center-items">
        <div class="col-lg-3 box-style1" @click="numberSelection('7')">7</div>
        <div class="col-lg-3 box-style1" @click="numberSelection('8')">8</div>
        <div class="col-lg-3 box-style1" @click="numberSelection('9')">9</div>
        <div class="col-lg-3 box-style2" @click="symbolSelection('*')">*</div>
      </div>
      <div class="row center-items">
        <div class="col-lg-3 box-style1" @click="numberSelection('4')">4</div>
        <div class="col-lg-3 box-style1" @click="numberSelection('5')">5</div>
        <div class="col-lg-3 box-style1" @click="numberSelection('6')">6</div>
        <div class="col-lg-3 box-style2" @click="symbolSelection('-')">-</div>
      </div>
      <div class="row center-items">
        <div class="col-lg-3 box-style1" @click="numberSelection('1')">1</div>
        <div class="col-lg-3 box-style1" @click="numberSelection('2')">2</div>
        <div class="col-lg-3 box-style1" @click="numberSelection('3')">3</div>
        <div class="col-lg-3 box-style2" @click="symbolSelection('+')">+</div>
      </div>
      <div class="row center-items">
        <div
          class="col-lg-6 box-style1 center-items zero-style"
          @click="numberSelection(0)"
        >
          0
        </div>
        <div class="col-lg-3 box-style1" @click="numberSelection('.')">.</div>
        <div class="col-lg-3 box-style2" @click="equal">=</div>
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
      printSelectedSymbol: "",
      number: "",
    };
  },
  methods: {
    numberSelection(selected) {
      this.number += selected;
      this.printSelectedNumber = this.number;
      console.log(this.number);
    },
    symbolSelection(selected) {
      this.selectedSymbols.push(selected);
      this.printSelectedSymbol = selected;
      if (this.number !== "") {
        this.selectedNumbers.push(parseFloat(this.number));
      }
      this.number = "";
      console.log(this.selectedNumbers);
    },
    equal() {
      parseFloat(this.number);
      this.selectedNumbers.push(parseFloat(this.number));
      this.number = "";
      if (this.selectedNumbers.length < 3) {
        let firstNumber = null;
        let secondNumber = null;
        for (let index = 0; index < this.selectedSymbols.length; index++) {
          firstNumber = this.selectedNumbers[0];
          secondNumber = this.selectedNumbers[1];
          this.selectedNumbers.splice(0, 2);
          this.selectedNumbers.unshift(
            this.test(firstNumber, secondNumber, this.selectedSymbols[index])
          );
          if (this.selectedNumbers.length === 1) {
            this.printSelectedNumber = this.selectedNumbers[0];
            this.selectedSymbols.splice(0, index + 1);
          }
        }
      } else {
        let firstNumber = this.selectedNumbers[0];
        let secondNumber = this.selectedNumbers[1];
        let symbol = this.selectedSymbols[0];
        this.selectedNumbers = [];
        this.selectedSymbols = [];
        this.selectedNumbers.unshift(
          this.test(firstNumber, secondNumber, symbol)
        );
      }
    },
    clearAll() {
      this.selectedNumbers = [];
      this.selectedSymbols = [];
      this.printSelectedNumber = null;
      this.printSelectedSymbol = null;
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
    /* dividetTimes() {
      if (this.selectedNumbers.length > 2) {
        let firstNumber = null;
        let secondNumber = null;
        let result = null;
        for (let i = 0; i < selectedSymbols.length; i++) {
          if (
            this.selectedSymbols[i] === "*" ||
            this.selectedSymbols[i] === "/"
          ) {
            firstNumber = this.selectedNumbers[i];
            secondNumber = this.selectedNumbers[i + 1];
            result = test(firstNumber, secondNumber, this.selectedSymbols[i]);
            this.selectedNumbers.splice(i, 2, result);
            this.selectedSymbols.splice(i, 1);
          } else {
          }
        }
      }
    }, */
  },
};
</script>

<style>
.calculator {
  height: 500px;
  color: white;
  font-size: 50px;
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
  height: 100px;
  cursor: pointer;
}
.box-style2 {
  background: #ed9421;
  border: none;
  border-radius: 50%;
  height: 100px;
  cursor: pointer;
}
.box-style3 {
  background: #a5a5a5;
  color: black;
  border: none;
  border-radius: 50%;
  height: 100px;
  cursor: pointer;
}
div .col-lg-3 {
  display: flex;
  justify-content: center;
  align-items: center;
}
.result {
  text-align: end;
  font-size: 60px;
}
.zero-style {
  border-radius: 50px;
}
.numbers-symbols {
  text-align: end;
  font-size: 12px;
}
</style>
