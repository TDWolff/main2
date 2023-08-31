---
toc: true
comments: false
layout: post
title: Calculator
description: This is a calculator made in .md format with JavaScript.
type: tangibles
courses: { compsci: {week: 1} }
---

<!-- 
Hack 1: Test conditions on small, big, and decimal numbers, report on findings. Fix issues.
Hack 2: Add a common math operation that is missing from the calculator.
Hack 3: Implement 1 number operation (i.e., SQRT) 
-->

<!-- 
HTML implementation of the calculator. 
-->

<div class="calculator-container">
    <!--result-->
    <div class="previous-output" id="previousOutput">Previous Output: </div>
    <div class="calculator-output" id="output">0</div>
    <!--row 1-->
    <div class="calculator-number">1</div>
    <div class="calculator-number">2</div>
    <div class="calculator-number">3</div>
    <div class="calculator-operation">+</div>
    <!--row 2-->
    <div class="calculator-number">4</div>
    <div class="calculator-number">5</div>
    <div class="calculator-number">6</div>
    <div class="calculator-operation">-</div>
    <!--row 3-->
    <div class="calculator-number">7</div>
    <div class="calculator-number">8</div>
    <div class="calculator-number">9</div>
    <div class="calculator-operation">*</div>
    <!--row 4-->
    <div class="calculator-clear">A/C</div>
    <div class="calculator-number">0</div>
    <div class="calculator-number">.</div>
    <div class="calculator-equals">=</div>
</div>

<!-- Style implementation for calculator -->
<style>
    /* Styles for calculator buttons */
    .calculator-number,
    .calculator-operation,
    .calculator-clear,
    .calculator-equals {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100px;
        height: 50px;
        border: 1px solid black;
        font-size: 18px;
        cursor: pointer;
        margin: 0px; /* Adjust this value to control horizontal spacing */
    }

    /* Additional styles for specific button types */
    .calculator-operation {
        background-color: orange;
        color: white;
    }

    .calculator-clear {
        background-color: red;
        color: white;
    }

    .calculator-equals {
        background-color: green;
        color: white;
    }

    .calculator-container {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        gap: 5px; /* Adjust this value to control spacing between buttons */
    }

    /* Other styles... */
    .calculator-output {
        grid-column: span 4;
        grid-row: span 1;
        border-radius: 5px;
        padding: 0.25em;
        font-size: 20px;
        border: 5px solid black;
        display: flex;
        align-items: center;
    }
</style>

<!-- JavaScript (JS) implementation of the calculator. -->
<script>
    // initialize important variables to manage calculations
    var firstNumber = null;
    var operator = null;
    var nextReady = true;
    // build objects containing key elements
    const output = document.getElementById("output");
    const previousOutput = document.getElementById("previousOutput");
    const numbers = document.querySelectorAll(".calculator-number");
    const operations = document.querySelectorAll(".calculator-operation");
    const clear = document.querySelectorAll(".calculator-clear");
    const equals = document.querySelectorAll(".calculator-equals");

    // Number buttons listener
    numbers.forEach(button => {
        button.addEventListener("click", function() {
            number(button.textContent);
        });
    });

    // Number action
    function number(value) {
        if (value != ".") {
            if (nextReady) {
                output.innerHTML = value;
                nextReady = false;
            } else {
                if (output.innerHTML === "0") {
                    output.innerHTML = value;
                } else {
                    output.innerHTML += value;
                }
            }
        } else {
            if (output.innerHTML.indexOf(".") == -1) {
                output.innerHTML += value;
            }
        }
    }

    // Operation buttons listener
    operations.forEach(button => {
        button.addEventListener("click", function() {
            operation(button.textContent);
        });
    });

    // Operator action
    function operation(choice) {
        if (firstNumber === null) {
            firstNumber = parseFloat(output.innerHTML);
            operator = choice;
            calculationString = firstNumber + " " + operator + " ";
            nextReady = true;
        } else {
            calculateAndUpdateOutput();
            operator = choice;
            calculationString += operator + " ";
        }
    }

    // Calculator
    function calculate(first, second) {
        let result = 0;
        switch (operator) {
            case "+":
                result = first + second;
                break;
            case "-":
                result = first - second;
                break;
            case "*":
                result = first * second;
                break;
            case "/":
                result = first / second;
                break;
            default:
                break;
        }
        return result;
    }

    // Calculate and update output
    function calculateAndUpdateOutput() {
        let secondNumber = parseFloat(output.innerHTML);
        let result = calculate(firstNumber, secondNumber);
        output.innerHTML = result.toString();
        firstNumber = result;
        nextReady = true;
    }

    // Initialize an array to store calculated results as a stack
    var resultsStack = [];

    // Equal action
    function equal() {
        if (firstNumber !== null && operator !== null) {
            let secondNumber = parseFloat(output.innerHTML);
            let result = calculate(firstNumber, secondNumber);
            resultsStack.push(result); // Push the result onto the stack
            output.innerHTML = "0";
            firstNumber = result;
            operator = null; // Reset the operator after calculation

                        // Display the last result in the "Previous Output" section
                    if (resultsStack.length > 0) {
                        previousOutput.textContent = `Previous Output: ${resultsStack[resultsStack.length - 1]}`;
                    }
                }
            }

            // Equals button listener
            equals.forEach(button => {
                button.addEventListener("click", function() {
                    equal();
                });
            });

            // Clear button listener
            clear.forEach(button => {
                button.addEventListener("click", function() {
                    clearCalc();
                });
            });

            // A/C action
            function clearCalc() {
                firstNumber = null;
                operator = null;
                output.innerHTML = "0";
                resultsStack = [];
                previousOutput.textContent = "Previous Output: ";
                nextReady = true;
            }
        </script>

