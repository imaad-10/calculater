<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Elegant Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'SF Pro Display', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            overflow: hidden;
        }

        .background-orbs {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .orb {
            position: absolute;
            border-radius: 50%;
            background: linear-gradient(45deg, rgba(255,255,255,0.1), rgba(255,255,255,0.05));
            filter: blur(1px);
            animation: float 20s infinite linear;
        }

        .orb:nth-child(1) {
            width: 200px;
            height: 200px;
            top: -100px;
            left: -100px;
            animation-delay: 0s;
        }

        .orb:nth-child(2) {
            width: 150px;
            height: 150px;
            top: 50%;
            right: -75px;
            animation-delay: -10s;
        }

        .orb:nth-child(3) {
            width: 300px;
            height: 300px;
            bottom: -150px;
            left: 30%;
            animation-delay: -5s;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(180deg); }
        }

        .calculator {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 24px;
            padding: 30px;
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.2);
            width: 360px;
            animation: slideIn 0.8s cubic-bezier(0.23, 1, 0.32, 1);
        }

        @keyframes slideIn {
            0% {
                opacity: 0;
                transform: translateY(30px) scale(0.95);
            }
            100% {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }

        .display {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 16px;
            padding: 25px;
            margin-bottom: 25px;
            min-height: 100px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            position: relative;
            overflow: hidden;
        }

        .display::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
        }

        .display-history {
            color: rgba(255, 255, 255, 0.6);
            font-size: 14px;
            font-weight: 400;
            margin-bottom: 5px;
            min-height: 20px;
        }

        .display-current {
            color: white;
            font-size: 36px;
            font-weight: 300;
            text-align: right;
            word-break: break-all;
            transition: all 0.2s ease;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
        }

        .btn {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 16px;
            color: white;
            font-size: 20px;
            font-weight: 500;
            height: 70px;
            cursor: pointer;
            transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
            user-select: none;
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(135deg, rgba(255,255,255,0.1), rgba(255,255,255,0.05));
            opacity: 0;
            transition: opacity 0.2s ease;
        }

        .btn:hover {
            transform: translateY(-2px) scale(1.02);
            background: rgba(255, 255, 255, 0.15);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
        }

        .btn:hover::before {
            opacity: 1;
        }

        .btn:active {
            transform: translateY(0) scale(0.98);
            transition: all 0.1s ease;
        }

        .btn.operator {
            background: linear-gradient(135deg, #ff6b6b, #ee5a52);
            border: 1px solid rgba(238, 90, 82, 0.3);
        }

        .btn.operator:hover {
            background: linear-gradient(135deg, #ff5252, #e04848);
            box-shadow: 0 10px 25px rgba(255, 107, 107, 0.4);
        }

        .btn.equals {
            background: linear-gradient(135deg, #4ecdc4, #26d0ce);
            border: 1px solid rgba(38, 208, 206, 0.3);
        }

        .btn.equals:hover {
            background: linear-gradient(135deg, #26d0ce, #1ab7b7);
            box-shadow: 0 10px 25px rgba(78, 205, 196, 0.4);
        }

        .btn.clear {
            background: linear-gradient(135deg, #feca57, #ff9ff3);
            border: 1px solid rgba(254, 202, 87, 0.3);
        }

        .btn.clear:hover {
            background: linear-gradient(135deg, #ff9ff3, #feca57);
            box-shadow: 0 10px 25px rgba(254, 202, 87, 0.4);
        }

        .btn.zero {
            grid-column: span 2;
        }

        .ripple {
            position: absolute;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.4);
            transform: scale(0);
            animation: ripple 0.6s linear;
        }

        @keyframes ripple {
            to {
                transform: scale(4);
                opacity: 0;
            }
        }

        .brand {
            text-align: center;
            color: rgba(255, 255, 255, 0.8);
            font-size: 12px;
            font-weight: 300;
            margin-top: 20px;
            letter-spacing: 2px;
        }

        @media (max-width: 400px) {
            .calculator {
                width: 100%;
                padding: 20px;
            }
            
            .display-current {
                font-size: 28px;
            }
            
            .btn {
                height: 60px;
                font-size: 18px;
            }
        }
    </style>
</head>
<body>
    <div class="background-orbs">
        <div class="orb"></div>
        <div class="orb"></div>
        <div class="orb"></div>
    </div>

    <div class="calculator">
        <div class="display">
            <div class="display-history" id="history"></div>
            <div class="display-current" id="display">0</div>
        </div>
        
        <div class="buttons">
            <button class="btn clear" onclick="clearDisplay()">AC</button>
            <button class="btn clear" onclick="deleteLast()">⌫</button>
            <button class="btn operator" onclick="appendOperator('/')">/</button>
            <button class="btn operator" onclick="appendOperator('*')">×</button>
            
            <button class="btn" onclick="appendNumber('7')">7</button>
            <button class="btn" onclick="appendNumber('8')">8</button>
            <button class="btn" onclick="appendNumber('9')">9</button>
            <button class="btn operator" onclick="appendOperator('-')">-</button>
            
            <button class="btn" onclick="appendNumber('4')">4</button>
            <button class="btn" onclick="appendNumber('5')">5</button>
            <button class="btn" onclick="appendNumber('6')">6</button>
            <button class="btn operator" onclick="appendOperator('+')">+</button>
            
            <button class="btn" onclick="appendNumber('1')">1</button>
            <button class="btn" onclick="appendNumber('2')">2</button>
            <button class="btn" onclick="appendNumber('3')">3</button>
            <button class="btn equals" onclick="calculate()" rowspan="2">=</button>
            
            <button class="btn zero" onclick="appendNumber('0')">0</button>
            <button class="btn" onclick="appendDecimal()">.</button>
        </div>
        
        <div class="brand">NEXUS CALC</div>
    </div>

    <script>
        let display = document.getElementById('display');
        let history = document.getElementById('history');
        let currentInput = '0';
        let previousInput = '';
        let operator = '';
        let shouldResetDisplay = false;

        function updateDisplay() {
            display.textContent = currentInput;
            display.style.transform = 'scale(1.05)';
            setTimeout(() => {
                display.style.transform = 'scale(1)';
            }, 100);
        }

        function updateHistory() {
            if (previousInput && operator) {
                history.textContent = `${previousInput} ${operator}`;
            } else {
                history.textContent = '';
            }
        }

        function appendNumber(num) {
            if (currentInput === '0' || shouldResetDisplay) {
                currentInput = num;
                shouldResetDisplay = false;
            } else {
                currentInput += num;
            }
            updateDisplay();
        }

        function appendDecimal() {
            if (shouldResetDisplay) {
                currentInput = '0';
                shouldResetDisplay = false;
            }
            if (currentInput.indexOf('.') === -1) {
                currentInput += '.';
                updateDisplay();
            }
        }

        function appendOperator(op) {
            if (previousInput && operator && !shouldResetDisplay) {
                calculate();
            }
            
            previousInput = currentInput;
            operator = op === '*' ? '×' : op;
            shouldResetDisplay = true;
            updateHistory();
        }

        function calculate() {
            if (previousInput && operator && currentInput) {
                let prev = parseFloat(previousInput);
                let current = parseFloat(currentInput);
                let result;
                
                const actualOperator = operator === '×' ? '*' : operator;
                
                switch (actualOperator) {
                    case '+':
                        result = prev + current;
                        break;
                    case '-':
                        result = prev - current;
                        break;
                    case '*':
                        result = prev * current;
                        break;
                    case '/':
                        result = current !== 0 ? prev / current : 'Error';
                        break;
                    default:
                        return;
                }
                
                if (result === 'Error') {
                    currentInput = 'Error';
                } else {
                    currentInput = result.toString();
                    if (currentInput.length > 12) {
                        currentInput = parseFloat(result).toExponential(6);
                    }
                }
                
                history.textContent = `${previousInput} ${operator} ${parseFloat(previousInput) !== prev ? previousInput : current} =`;
                previousInput = '';
                operator = '';
                shouldResetDisplay = true;
                updateDisplay();
            }
        }

        function clearDisplay() {
            currentInput = '0';
            previousInput = '';
            operator = '';
            history.textContent = '';
            shouldResetDisplay = false;
            updateDisplay();
        }

        function deleteLast() {
            if (currentInput.length > 1) {
                currentInput = currentInput.slice(0, -1);
            } else {
                currentInput = '0';
            }
            updateDisplay();
        }

        // Add ripple effect to buttons
        document.querySelectorAll('.btn').forEach(button => {
            button.addEventListener('click', function(e) {
                let ripple = document.createElement('span');
                ripple.classList.add('ripple');
                
                let rect = button.getBoundingClientRect();
                let size = Math.max(rect.width, rect.height);
                let x = e.clientX - rect.left - size / 2;
                let y = e.clientY - rect.top - size / 2;
                
                ripple.style.width = ripple.style.height = size + 'px';
                ripple.style.left = x + 'px';
                ripple.style.top = y + 'px';
                
                button.appendChild(ripple);
                
                setTimeout(() => {
                    ripple.remove();
                }, 600);
            });
        });

        // Keyboard support
        document.addEventListener('keydown', function(e) {
            if (e.key >= '0' && e.key <= '9') {
                appendNumber(e.key);
            } else if (e.key === '.') {
                appendDecimal();
            } else if (e.key === '+') {
                appendOperator('+');
            } else if (e.key === '-') {
                appendOperator('-');
            } else if (e.key === '*') {
                appendOperator('*');
            } else if (e.key === '/') {
                e.preventDefault();
                appendOperator('/');
            } else if (e.key === 'Enter' || e.key === '=') {
                calculate();
            } else if (e.key === 'Escape') {
                clearDisplay();
            } else if (e.key === 'Backspace') {
                deleteLast();
            }
        });

        // Initialize display
        updateDisplay();
    </script>
</body>
</html>
