
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scientific Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: #111;
            color: #333;
            padding: 20px;
        }
        
        .calculator {
            background: #fff;
            border-radius: 16px;
            padding: 25px;
            box-shadow: 0 0 30px rgba(80, 80, 255, 0.3);
            max-width: 420px;
            width: 100%;
            border: 1px solid #ddd;
            margin-bottom: 20px;
        }
        
        .logo {
            text-align: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid #eee;
        }
        
        .logo img {
            max-width: 180px;
            height: auto;
        }
        
        #display {
            width: 100%;
            height: 80px;
            font-size: 32px;
            text-align: right;
            margin-bottom: 20px;
            padding: 20px;
            border: 2px solid #e0e0e0;
            border-radius: 12px;
            background: #f9f9f9;
            color: #333;
            outline: none;
        }
        
        .buttons {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 12px;
        }
        
        button {
            height: 55px;
            font-size: 18px;
            border: 1px solid #e0e0e0;
            border-radius: 10px;
            background: #f5f5f5;
            color: #333;
            cursor: pointer;
            transition: all 0.15s;
            font-weight: 500;
        }
        
        button:hover {
            background: #e8e8e8;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        
        button:active {
            transform: translateY(0);
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .number {
            background: #fff;
            border: 1px solid #ddd;
        }
        
        .number:hover {
            background: #f0f0f0;
        }
        
        .operator {
            background: #4a7cff;
            color: white;
            border: none;
        }
        
        .operator:hover {
            background: #3a6cef;
        }
        
        .function {
            background: #f0f0f0;
            font-size: 16px;
            border: 1px solid #ddd;
        }
        
        .function:hover {
            background: #e0e0e0;
        }
        
        .equals {
            background: #2ecc71;
            color: white;
            grid-column: span 2;
            border: none;
        }
        
        .equals:hover {
            background: #27ae60;
        }
        
        .clear {
            background: #e74c3c;
            color: white;
            border: none;
        }
        
        .clear:hover {
            background: #c0392b;
        }
        
        .mode-toggle {
            margin-top: 15px;
            text-align: center;
        }
        
        .mode-toggle button {
            background: #4a7cff;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 14px;
            border: none;
        }
        
        .mode-toggle button:hover {
            background: #3a6cef;
        }
        
        .footer {
            text-align: center;
            color: white;
            font-size: 18px;
            margin-top: 10px;
            padding: 15px;
            font-family: 'Segoe UI', sans-serif;
        }
        
        .heart {
            color: #e74c3c;
            display: inline-block;
            animation: heartbeat 1.5s ease-in-out infinite both;
        }
        
        @keyframes heartbeat {
            from {
                transform: scale(1);
                transform-origin: center center;
                animation-timing-function: ease-out;
            }
            10% {
                transform: scale(0.91);
                animation-timing-function: ease-in;
            }
            17% {
                transform: scale(0.98);
                animation-timing-function: ease-out;
            }
            33% {
                transform: scale(0.87);
                animation-timing-function: ease-in;
            }
            45% {
                transform: scale(1);
                animation-timing-function: ease-out;
            }
        }
        
        @media (max-width: 480px) {
            .calculator {
                padding: 15px;
                max-width: 95%;
            }
            
            button {
                height: 50px;
                font-size: 16px;
            }
            
            #display {
                height: 70px;
                font-size: 28px;
                padding: 15px;
            }
            
            .footer {
                font-size: 16px;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="logo">
            <img src="https://i.ibb.co/dsTTy0dd/image.png" alt="Calculator Logo">
        </div>
        
        <input type="text" id="display" readonly placeholder="0">
        
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="clear" onclick="backspace()">⌫</button>
            <button class="function" onclick="appendToDisplay('(')">(</button>
            <button class="function" onclick="appendToDisplay(')')">)</button>
            <button class="function" onclick="appendConstant('π')">π</button>
            
            <button class="function" onclick="appendFunction('sin(')">sin</button>
            <button class="function" onclick="appendFunction('cos(')">cos</button>
            <button class="function" onclick="appendFunction('tan(')">tan</button>
            <button class="function" onclick="appendFunction('log(')">log</button>
            <button class="function" onclick="appendFunction('√(')">√</button>
            
            <button class="number" onclick="appendToDisplay('7')">7</button>
            <button class="number" onclick="appendToDisplay('8')">8</button>
            <button class="number" onclick="appendToDisplay('9')">9</button>
            <button class="operator" onclick="appendToDisplay('/')">/</button>
            <button class="function" onclick="appendToDisplay('^')">^</button>
            
            <button class="number" onclick="appendToDisplay('4')">4</button>
            <button class="number" onclick="appendToDisplay('5')">5</button>
            <button class="number" onclick="appendToDisplay('6')">6</button>
            <button class="operator" onclick="appendToDisplay('*')">×</button>
            <button class="function" onclick="appendConstant('e')">e</button>
            
            <button class="number" onclick="appendToDisplay('1')">1</button>
            <button class="number" onclick="appendToDisplay('2')">2</button>
            <button class="number" onclick="appendToDisplay('3')">3</button>
            <button class="operator" onclick="appendToDisplay('-')">-</button>
            <button class="function" onclick="appendFunction('ln(')">ln</button>
            
            <button class="number" onclick="appendToDisplay('0')">0</button>
            <button class="number" onclick="appendToDisplay('.')">.</button>
            <button class="equals" onclick="calculate()">=</button>
            <button class="operator" onclick="appendToDisplay('+')">+</button>
        </div>
        
        <div class="mode-toggle">
            <button onclick="toggleScientificMode()">Scientific Mode</button>
        </div>
    </div>
    
    <div class="footer">
        Made in <span class="heart">❤️</span> By Armeen
    </div>

    <script>
        let display = document.getElementById('display');
        let isScientificMode = true;
        
        function appendToDisplay(value) {
            if (display.value === '0' || display.value === 'Error') {
                display.value = value;
            } else {
                display.value += value;
            }
        }
        
        function appendFunction(func) {
            if (display.value === '0' || display.value === 'Error' || display.value === '') {
                display.value = func;
            } else {
                display.value += func;
            }
        }
        
        function appendConstant(constant) {
            const constants = {
                'π': Math.PI,
                'e': Math.E
            };
            
            if (display.value === '0' || display.value === 'Error' || display.value === '') {
                display.value = constants[constant];
            } else {
                display.value += constants[constant];
            }
        }
        
        function clearDisplay() {
            display.value = '';
        }
        
        function backspace() {
            display.value = display.value.slice(0, -1);
            if (display.value === '') {
                display.value = '';
            }
        }
        
        function calculate() {
            let expression = display.value;
            
            // Replace display operators
            expression = expression.replace(/×/g, '*');
            
            // Handle power operator
            expression = expression.replace(/\^/g, '**');
            
            // Handle functions
            expression = expression.replace(/sin\(/g, 'Math.sin(');
            expression = expression.replace(/cos\(/g, 'Math.cos(');
            expression = expression.replace(/tan\(/g, 'Math.tan(');
            expression = expression.replace(/log\(/g, 'Math.log10(');
            expression = expression.replace(/ln\(/g, 'Math.log(');
            expression = expression.replace(/√\(/g, 'Math.sqrt(');
            
            try {
                let result = Function('"use strict"; return (' + expression + ')')();
                
                // Handle very large or very small numbers
                if (Math.abs(result) > 1e10 || (Math.abs(result) < 1e-6 && result !== 0)) {
                    display.value = result.toExponential(6);
                } else {
                    display.value = parseFloat(result.toPrecision(10));
                }
            } catch (error) {
                display.value = 'Error';
            }
        }
        
        function toggleScientificMode() {
            const scientificButtons = document.querySelectorAll('.function');
            const toggleBtn = document.querySelector('.mode-toggle button');
            
            isScientificMode = !isScientificMode;
            
            if (isScientificMode) {
                scientificButtons.forEach(btn => btn.style.display = 'block');
                toggleBtn.textContent = 'Basic Mode';
            } else {
                scientificButtons.forEach(btn => btn.style.display = 'none');
                toggleBtn.textContent = 'Scientific Mode';
            }
        }
        
        // Keyboard support
        document.addEventListener('keydown', function(event) {
            const key = event.key;
            
            if (/[0-9+\-*/.=]/.test(key)) {
                event.preventDefault();
                if (key === '=' || key === 'Enter') {
                    calculate();
                } else {
                    appendToDisplay(key);
                }
            }
            
            else if (key === '(' || key === ')') {
                event.preventDefault();
                appendToDisplay(key);
            }
            
            else if (key === 'Backspace') {
                event.preventDefault();
                backspace();
            }
            
            else if (key === 'Escape') {
                event.preventDefault();
                clearDisplay();
            }
        });
    </script>
</body>
</html>
