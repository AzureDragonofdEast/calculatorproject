//JavaScript codes

var calculate = function (n1, operator, n2) {
  var result = '';
  if (operator === 'add') {
    result = parseFloat(n1) + parseFloat(n2);
  } else if (operator === 'subtract') {
    result = parseFloat(n1) - parseFloat(n2);
  } else if (operator === 'multiply') {
    result = parseFloat(n1) * parseFloat(n2);
  } else if (operator === 'divide') {
    result = parseFloat(n1) / parseFloat(n2);
  }

  return result;
}

var calculator = document.querySelector('.calculator');
var display = calculator.querySelector('.calculator__display');
var keys = calculator.querySelector('.calculator__keys');

keys.addEventListener('click', function(e) {
  if (e.target.matches('button')) {
    var key = e.target;
    var action = key.dataset.action;
    var keyContent = key.textContent;
    var displayedNum = display.textContent;
    var previousKeyType = calculator.dataset.previousKeyType;

    Array.from(key.parentNode.children)
      .forEach(function(k){ k.classList.remove('is-depressed')});

    if (!action) {
      if ( displayedNum === '0' || previousKeyType === 'operator' || previousKeyType === 'calculate' ) {
        display.textContent = keyContent;
      } else {
        display.textContent = displayedNum + keyContent;
      }
      calculator.dataset.previousKeyType = 'number';
    }

    if (action === 'decimal') {
      if (!displayedNum.includes('.')) {
        display.textContent = displayedNum + '.';
      } else if ( previousKeyType === 'operator' || previousKeyType === 'calculate' ) {
        display.textContent = '0.';
      }

      calculator.dataset.previousKeyType = 'decimal';
    }

    if ( action === 'add' || action === 'subtract' || action === 'multiply' || action === 'divide' ) {
      var firstValue = calculator.dataset.firstValue;
      var operator = calculator.dataset.operator;
      var secondValue = displayedNum;

      if ( firstValue && operator && previousKeyType !== 'operator' && previousKeyType !== 'calculate' ) {
        var calcValue = calculate(firstValue, operator, secondValue);
        display.textContent = calcValue;
        calculator.dataset.firstValue = calcValue;
      } else {
        calculator.dataset.firstValue = displayedNum;
      }

      key.classList.add('is-depressed');
      calculator.dataset.previousKeyType = 'operator';
      calculator.dataset.operator = action;
    }

    if (action === 'clear') {
      if (key.textContent === 'AC') {
        calculator.dataset.firstValue = '';
        calculator.dataset.modValue = '';
        calculator.dataset.operator = '';
        calculator.dataset.previousKeyType = '';
      } else {
        key.textContent = 'AC';
      }

      display.textContent = 0;
      calculator.dataset.previousKeyType = 'clear';
    }

    if (action !== 'clear') {
      var clearButton = calculator.querySelector('[data-action=clear]');
      clearButton.textContent = 'CE';
    }

    if (action === 'calculate') {
      var firstValue = calculator.dataset.firstValue;
      var operator = calculator.dataset.operator;
      var secondValue = displayedNum;

      if (firstValue) {
        if (previousKeyType === 'calculate') {
          firstValue = displayedNum;
          secondValue = calculator.dataset.modValue;
        }

        display.textContent = calculate(firstValue, operator, secondValue);
      }

      calculator.dataset.modValue = secondValue;
      calculator.dataset.previousKeyType = 'calculate';
    }
  }
})
