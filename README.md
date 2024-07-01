<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Basic Calculator</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            font-family: Arial, sans-serif;
        }
        .calculator {
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            width: 300px;
        }
        .calculator-screen {
            width: 100%;
            height: 80px;
            border: none;
            background-color: #252525;
            color: #fff;
            text-align: right;
            padding: 20px;
            font-size: 2.5rem;
            box-sizing: border-box;
        }
        .calculator-keys {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            grid-gap: 10px;
            padding: 20px;
        }
        .calculator-keys button {
            height: 60px;
            font-size: 1.5rem;
            border: none;
            border-radius: 5px;
            background-color: #f1f1f1;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .calculator-keys button:hover {
            background-color: #eaeaea;
        }
        .calculator-keys button.operator {
            background-color: #ff9500;
            color: #fff;
        }
        .calculator-keys button.operator:hover {
            background-color: #ff8500;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <input type="text" class="calculator-screen" disabled />
        <div class="calculator-keys">
            <button type="button" class="operator" value="+">+</button>
            <button type="button" class="operator" value="-">-</button>
            <button type="button" class="operator" value="*">&times;</button>
            <button type="button" class="operator" value="/">&divide;</button>
            <button type="button" value="7">7</button>
            <button type="button" value="8">8</button>
            <button type="button" value="9">9</button>
            <button type="button" value="4">4</button>
            <button type="button" value="5">5</button>
            <button type="button" value="6">6</button>
            <button type="button" value="1">1</button>
            <button type="button" value="2">2</button>
            <button type="button" value="3">3</button>
            <button type="button" value="0">0</button>
            <button type="button" value=".">.</button>
            <button type="button" class="operator" value="=">=</button>
            <button type="button" class="all-clear" value="all-clear">AC</button>
        </div>
    </div>

    <script>
        const calculator = {
            displayValue: '0',
            firstOperand: null,
            waitingForSecondOperand: false,
            operator: null,
        };

        function updateDisplay() {
            const display = document.querySelector('.calculator-screen');
            display.value = calculator.displayValue;
        }

        updateDisplay();

        const keys = document.querySelector('.calculator-keys');
        keys.addEventListener('click', event => {
            const { target } = event;
            const { value } = target;
            if (!target.matches('button')) {
                return;
            }

            switch (value) {
                case '+':
                case '-':
                case '*':
                case '/':
                case '=':
                    handleOperator(value);
                    break;
                case '.':
                    inputDecimal(value);
                    break;
                case 'all-clear':
                    resetCalculator();
                    break;
                default:
                    if (Number.isInteger(parseFloat(value))) {
                        inputDigit(value);
                    }
            }

            updateDisplay();
        });

        function inputDigit(digit) {
            const { displayValue, waitingForSecondOperand } = calculator;

            if (waitingForSecondOperand === true) {
                calculator.displayValue = digit;
                calculator.waitingForSecondOperand = false;
            } else {
                calculator.displayValue = displayValue === '0' ? digit : displayValue + digit;
            }
        }

        function inputDecimal(dot) {
            if (calculator.waitingForSecondOperand === true) return;

            if (!calculator.displayValue.includes(dot)) {
                calculator.displayValue += dot;
            }
        }

        function handleOperator(nextOperator) {
            const { firstOperand, displayValue, operator } = calculator;
            const inputValue = parseFloat(displayValue);

            if (operator && calculator.waitingForSecondOperand)  {
                calculator.operator = nextOperator;
                return;
            }

            if (firstOperand == null) {
                calculator.firstOperand = inputValue;
            } else if (operator) {
                const currentValue = firstOperand || 0;
                const result = performCalculation[operator](currentValue, inputValue);

                calculator.displayValue = String(result);
                calculator.firstOperand = result;
            }

            calculator.waitingForSecondOperand = true;
            calculator.operator = nextOperator;
        }

        const performCalculation = {
            '/': (firstOperand, secondOperand) => firstOperand / secondOperand,
            '*': (firstOperand, secondOperand) => firstOperand * secondOperand,
            '+': (firstOperand, secondOperand) => firstOperand + secondOperand,
            '-': (firstOperand, secondOperand) => firstOperand - secondOperand,
            '=': (firstOperand, secondOperand) => secondOperand
        };

        function resetCalculator() {
            calculator.displayValue = '0';
            calculator.firstOperand = null;
            calculator.waitingForSecondOperand = false;
            calculator.operator = null;
        }
    </script>
</body>
</html>
