# Calculator
with these codes (icludes html, css, js) you can make your own Calculator ... 

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator</title>
</head>
<style>
    *, *::before, *::after {
  box-sizing: border-box;
  font-family: Gotham Rounded, sans-serif;
  font-weight: normal;
}
body {
  margin: 0;
  padding: 0;
  background: linear-gradient(to right, rgb(206, 212, 93), rgb(172, 54, 80));
  }
  .calculator-grid {
    display: grid;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    grid-template-columns: repeat(4, 100px);
    grid-template-rows: minmax(120px, auto) repeat(5, 100px);
  }
  .calculator-grid button {
    cursor: pointer;
    font-family: 'Times New Roman', Times, serif;
    font-size: 2.6rem;
    border: 2px solid rgb(236, 72, 72);
    outline: none;
    background-color: rbga(255, 255, 255, 0.75);
    height: 100px;
  }
  .calculator-grid button:hover {
background-color: rgb(211, 53, 105);
  }
  .span-twe {
    grid-column: span 2;
    color: rgb(47, 109, 241);
    background-color: rgba(82, 245, 182, 0.85);
  }
  .output {
    grid-column: span 4;
    background-color: rgb(168, 172, 164);
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    flex-direction: column;
    padding: 5px;
    word-wrap: break-word;
    word-break: break-all;
    border: 1px solid black;
    height: 120px;
  }
.output .previous-operand {
    color: rgba(27, 26, 26, 0.75);
    font-size: 1.5rem;
    font-family: 'Times New Roman', Times, serif;
}
.output .current-operand{
    color: rgb(27, 26, 26);
    font-size: 2.6rem;
    font-family: 'Times New Roman', Times, serif;
}
</style>
<body>
    <div class="calculator-grid">
        <div class="output">
            <div data-previous-operand class="previous-operand"></div>
            <div data-current-operand class="current-operand"></div>
        </div>
        <button data-all-clear class="span-twe">AC</button>
        <button data-delete>DEL</button>
        <button data-operation>??</button>
        <button data-number>1</button>
        <button data-number>2</button>
        <button data-number>3</button>
        <button data-operation>*</button>
        <button data-number>4</button>
        <button data-number>5</button>
        <button data-number>6</button>
        <button data-operation>+</button>
        <button data-number>7</button>
        <button data-number>8</button>
        <button data-number>9</button>
        <button data-operation>-</button>
        <button data-number>.</button>
        <button data-number>0</button>
        <button data-equals class="span-twe">=</button>
    </div>
    <script>
class Calculator {
  constructor(previousOperandTextElement, currentOperandTextElement) {
    this.previousOperandTextElement = previousOperandTextElement
    this.currentOperandTextElement = currentOperandTextElement
    this.clear()
  }

  clear() {
    this.currentOperand = ''
    this.previousOperand = ''
    this.operation = undefined
  }

  delete() {
    this.currentOperand = this.currentOperand.toString().slice(0, -1)
  }

  appendNumber(number) {
    if (number === '.' && this.currentOperand.includes('.')) return
    this.currentOperand = this.currentOperand.toString() + number.toString()
  }

  chooseOperation(operation) {
    if (this.currentOperand === '') return
    if (this.previousOperand !== '') {
      this.compute()
    }
    this.operation = operation
    this.previousOperand = this.currentOperand
    this.currentOperand = ''
  }

  compute() {
    let computation
    const prev = parseFloat(this.previousOperand)
    const current = parseFloat(this.currentOperand)
    if (isNaN(prev) || isNaN(current)) return
    switch (this.operation) {
      case '+':
        computation = prev + current
        break
      case '-':
        computation = prev - current
        break
      case '*':
        computation = prev * current
        break
      case '??':
        computation = prev / current
        break
      default:
        return
    }
    this.currentOperand = computation
    this.operation = undefined
    this.previousOperand = ''
  }

  getDisplayNumber(number) {
    const stringNumber = number.toString()
    const integerDigits = parseFloat(stringNumber.split('.')[0])
    const decimalDigits = stringNumber.split('.')[1]
    let integerDisplay
    if (isNaN(integerDigits)) {
      integerDisplay = ''
    } else {
      integerDisplay = integerDigits.toLocaleString('en', { maximumFractionDigits: 0 })
    }
    if (decimalDigits != null) {
      return `${integerDisplay}.${decimalDigits}`
    } else {
      return integerDisplay
    }
  }

  updateDisplay() {
    this.currentOperandTextElement.innerText =
      this.getDisplayNumber(this.currentOperand)
    if (this.operation != null) {
      this.previousOperandTextElement.innerText =
        `${this.getDisplayNumber(this.previousOperand)} ${this.operation}`
    } else {
      this.previousOperandTextElement.innerText = ''
    }
  }
}

        const numberButtons = document.querySelectorAll('[data-number]')
        const operationButtons = document.querySelectorAll('[data-operation]')
        const deleteButton = document.querySelector('[data-delete]')
        const equalsButton = document.querySelector('[data-equals]')
        const allClearButton = document.querySelector('[data-all-clear]')
        const previousOperandTextElement = document.querySelector('[data-previous-operand]')
        const currentOperandTextElement = document.querySelector('[data-current-operand]')

        const calculator = new Calculator(previousOperandTextElement, currentOperandTextElement)

numberButtons.forEach(button => {
  button.addEventListener('click', () => {
    calculator.appendNumber(button.innerText)
    calculator.updateDisplay()
  })
})

operationButtons.forEach(button => {
  button.addEventListener('click', () => {
    calculator.chooseOperation(button.innerText)
    calculator.updateDisplay()
  })
})

equalsButton.addEventListener('click', button => {
  calculator.compute()
  calculator.updateDisplay()
})

allClearButton.addEventListener('click', button => {
  calculator.clear()
  calculator.updateDisplay()
})

deleteButton.addEventListener('click', button => {
  calculator.delete()
  calculator.updateDisplay()
})

document.addEventListener('keydown', function (event) {
  let patternForNumbers = /[0-9]/g;
  let patternForOperators = /[+\-*\/]/g
  if (event.key.match(patternForNumbers)) {
    event.preventDefault();
    calculator.appendNumber(event.key)
    calculator.updateDisplay()
  }
  if (event.key === '.') {
    event.preventDefault();
    calculator.appendNumber(event.key)
    calculator.updateDisplay()
  }
  if (event.key.match(patternForOperators)) {
    event.preventDefault();
    calculator.chooseOperation(event.key)
    calculator.updateDisplay()
  }
  if (event.key === 'Enter' || event.key === '=') {
    event.preventDefault();
    calculator.compute()
    calculator.updateDisplay()
  }
  if (event.key === "Backspace") {
    event.preventDefault();
    calculator.delete()
    calculator.updateDisplay()
  }
  if (event.key == 'Delete') {
    event.preventDefault();
    calculator.clear()
    calculator.updateDisplay()
   }

});

    </script>
</body>
</html>

