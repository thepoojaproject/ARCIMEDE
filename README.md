<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#000000">
    <title>Scientific Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
            -webkit-text-size-adjust: none;
        }
        
        html, body {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background: #000000;
            touch-action: manipulation;
        }
        
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 0;
            margin: 0;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
        }
        
        .calculator {
            background-color: #000000;
            width: 100%;
            height: 100%;
            overflow: hidden;
            padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
            display: flex;
            flex-direction: column;
            position: relative;
        }
        
        .calculator-inner {
            background-color: #fff;
            flex: 1;
            display: flex;
            flex-direction: column;
            padding: 15px;
            margin: 0;
            border-radius: 0;
        }
        
        .header {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid #eaeaea;
            flex-shrink: 0;
        }
        
        .logo {
            height: 35px;
            max-width: 100%;
        }
        
        .display-container {
            flex-shrink: 0;
            margin-bottom: 15px;
        }
        
        .display {
            background-color: #f8f9fa;
            border-radius: 12px;
            padding: 20px 15px;
            text-align: right;
            color: #333;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.1);
            border: 2px solid #e9ecef;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            user-select: none;
            -webkit-user-select: none;
        }
        
        .display:active {
            background-color: #f0f3f5;
            transform: scale(0.995);
        }
        
        .previous-operand {
            font-size: 1.1rem;
            color: #666;
            min-height: 1.4rem;
            word-wrap: break-word;
            word-break: break-all;
        }
        
        .current-operand {
            font-size: 2.5rem;
            font-weight: 600;
            word-wrap: break-word;
            word-break: break-all;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .buttons-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            min-height: 0;
        }
        
        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-gap: 10px;
            flex: 1;
        }
        
        button {
            border: none;
            border-radius: 12px;
            font-size: 1.3rem;
            cursor: pointer;
            transition: all 0.1s ease;
            color: #333;
            background-color: #f0f0f0;
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
            font-weight: 500;
            touch-action: manipulation;
            min-height: 60px;
            user-select: none;
            -webkit-user-select: none;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        button:active {
            transform: scale(0.92);
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
        }
        
        .operation {
            background-color: #e9ecef;
            color: #495057;
            font-weight: 600;
        }
        
        .operation:active {
            background-color: #dde1e6;
        }
        
        .equals {
            background-color: #007bff;
            color: white;
            font-weight: 600;
        }
        
        .equals:active {
            background-color: #0069d9;
        }
        
        .scientific {
            background-color: #f8f9fa;
            font-size: 1rem;
            font-weight: 500;
        }
        
        .scientific:active {
            background-color: #e9ecef;
        }
        
        .clear {
            background-color: #f8f9fa;
            color: #dc3545;
            font-weight: 600;
        }
        
        .clear:active {
            background-color: #f1f3f4;
        }
        
        .zero {
            grid-column: span 2;
        }
        
        .footer-area {
            margin-top: 15px;
            flex-shrink: 0;
        }
        
        .mode-toggle {
            background-color: #333;
            color: white;
            font-size: 1.1rem;
            width: 100%;
            padding: 16px;
            border-radius: 12px;
            transition: all 0.3s ease;
            border: none;
            min-height: 55px;
            font-weight: 600;
        }
        
        .mode-toggle:active {
            background-color: #555;
            transform: scale(0.98);
        }
        
        .footer {
            text-align: center;
            margin-top: 10px;
            color: #000000; /* Changed from white to black */
            font-size: 0.9rem;
            padding: 5px;
            font-weight: 500;
        }
        
        .error {
            color: #dc3545;
        }
        
        /* Firework particles */
        .particle {
            position: absolute;
            width: 6px;
            height: 6px;
            border-radius: 50%;
            pointer-events: none;
            z-index: 1000;
        }
        
        /* Landscape orientation support */
        @media (orientation: landscape) and (max-height: 500px) {
            .calculator-inner {
                padding: 10px;
            }
            
            .header {
                margin-bottom: 10px;
                padding-bottom: 5px;
            }
            
            .display-container {
                margin-bottom: 10px;
            }
            
            .display {
                padding: 15px 12px;
                min-height: 80px;
            }
            
            .current-operand {
                font-size: 2rem;
            }
            
            .previous-operand {
                font-size: 1rem;
            }
            
            button {
                min-height: 45px;
                font-size: 1.1rem;
            }
            
            .scientific {
                font-size: 0.9rem;
            }
            
            .buttons-grid {
                grid-gap: 8px;
            }
            
            .mode-toggle {
                padding: 12px;
                min-height: 45px;
                font-size: 1rem;
            }
        }
        
        /* Very small screens */
        @media (max-height: 600px) {
            .display {
                min-height: 100px;
                padding: 15px;
            }
            
            .current-operand {
                font-size: 2.2rem;
            }
            
            button {
                min-height: 50px;
            }
        }
        
        /* Prevent text selection */
        .no-select {
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            -khtml-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
        }
        
        /* Safe area support for notched devices */
        @supports(padding: max(0px)) {
            .calculator {
                padding-left: max(15px, env(safe-area-inset-left));
                padding-right: max(15px, env(safe-area-inset-right));
                padding-top: max(15px, env(safe-area-inset-top));
                padding-bottom: max(15px, env(safe-area-inset-bottom));
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="calculator-inner">
            <div class="header">
                <img src="https://i.ibb.co/dsTTy0dd/image.png" alt="Calculator Logo" class="logo">
            </div>
            
            <div class="display-container">
                <div class="display" id="display">
                    <div class="previous-operand"></div>
                    <div class="current-operand">0</div>
                </div>
            </div>
            
            <div class="buttons-container">
                <div class="buttons-grid" id="buttons-grid">
                    <!-- Row 1: Memory and clear functions -->
                    <button class="scientific no-select" data-operation="mc">MC</button>
                    <button class="scientific no-select" data-operation="mr">MR</button>
                    <button class="scientific no-select" data-operation="m+">M+</button>
                    <button class="scientific no-select" data-operation="m-">M-</button>
                    <button class="clear no-select" data-action="clear">C</button>
                    
                    <!-- Row 2: Scientific functions -->
                    <button class="scientific no-select" data-operation="sin">sin</button>
                    <button class="scientific no-select" data-operation="cos">cos</button>
                    <button class="scientific no-select" data-operation="tan">tan</button>
                    <button class="scientific no-select" data-operation="π">π</button>
                    <button class="clear no-select" data-action="clear-entry">CE</button>
                    
                    <!-- Row 3: Scientific functions -->
                    <button class="scientific no-select" data-operation="log">log</button>
                    <button class="scientific no-select" data-operation="ln">ln</button>
                    <button class="scientific no-select" data-operation="x²">x²</button>
                    <button class="scientific no-select" data-operation="√">√</button>
                    <button class="scientific no-select" data-action="backspace">⌫</button>
                    
                    <!-- Row 4: Numbers and operations -->
                    <button class="scientific no-select" data-operation="x^y">x^y</button>
                    <button class="scientific no-select" data-operation="10^x">10^x</button>
                    <button class="number no-select" data-number="7">7</button>
                    <button class="number no-select" data-number="8">8</button>
                    <button class="number no-select" data-number="9">9</button>
                    
                    <!-- Row 5: Numbers and operations -->
                    <button class="scientific no-select" data-operation="1/x">1/x</button>
                    <button class="scientific no-select" data-operation="%">%</button>
                    <button class="number no-select" data-number="4">4</button>
                    <button class="number no-select" data-number="5">5</button>
                    <button class="number no-select" data-number="6">6</button>
                    
                    <!-- Row 6: Numbers and operations -->
                    <button class="scientific no-select" data-operation="e">e</button>
                    <button class="scientific no-select" data-operation="±">±</button>
                    <button class="number no-select" data-number="1">1</button>
                    <button class="number no-select" data-number="2">2</button>
                    <button class="number no-select" data-number="3">3</button>
                    
                    <!-- Row 7: Numbers and operations -->
                    <button class="operation no-select" data-operation="(">(</button>
                    <button class="operation no-select" data-operation=")">)</button>
                    <button class="number zero no-select" data-number="0">0</button>
                    <button class="number no-select" data-number=".">.</button>
                    <button class="equals no-select" data-action="calculate">=</button>
                    
                    <!-- Row 8: Basic operations (right side) -->
                    <button class="operation no-select" data-operation="÷">÷</button>
                    <button class="operation no-select" data-operation="×">×</button>
                    <button class="operation no-select" data-operation="-">-</button>
                    <button class="operation no-select" data-operation="+">+</button>
                </div>
            </div>
            
            <div class="footer-area">
                <button class="mode-toggle no-select" id="mode-toggle">Switch to Basic Mode</button>
                <div class="footer">
                    Made in 💖 By Armeen
                </div>
            </div>
        </div>
    </div>

    <script>
        class Calculator {
            constructor(previousOperandElement, currentOperandElement) {
                this.previousOperandElement = previousOperandElement;
                this.currentOperandElement = currentOperandElement;
                this.clear();
                this.isScientificMode = true;
                this.memory = 0;
            }
            
            clear() {
                this.currentOperand = '0';
                this.previousOperand = '';
                this.operation = undefined;
                this.shouldResetScreen = false;
                this.hasError = false;
            }
            
            clearEntry() {
                this.currentOperand = '0';
                this.hasError = false;
            }
            
            delete() {
                if (this.hasError) {
                    this.clear();
                    return;
                }
                
                if (this.currentOperand.length === 1) {
                    this.currentOperand = '0';
                } else {
                    this.currentOperand = this.currentOperand.slice(0, -1);
                }
            }
            
            appendNumber(number) {
                if (this.hasError) {
                    this.clear();
                }
                
                if (this.shouldResetScreen) {
                    this.currentOperand = '';
                    this.shouldResetScreen = false;
                }
                
                if (number === '.' && this.currentOperand.includes('.')) return;
                
                if (this.currentOperand === '0' && number !== '.') {
                    this.currentOperand = number;
                } else {
                    this.currentOperand += number;
                }
            }
            
            chooseOperation(operation) {
                if (this.hasError) return;
                
                if (this.currentOperand === '') return;
                
                if (this.previousOperand !== '') {
                    this.calculate();
                }
                
                this.operation = operation;
                this.previousOperand = this.currentOperand;
                this.shouldResetScreen = true;
            }
            
            calculate() {
                if (this.hasError) return;
                
                let computation;
                const prev = parseFloat(this.previousOperand);
                const current = parseFloat(this.currentOperand);
                
                if (isNaN(prev) || isNaN(current)) return;
                
                try {
                    switch (this.operation) {
                        case '+':
                            computation = prev + current;
                            break;
                        case '-':
                            computation = prev - current;
                            break;
                        case '×':
                            computation = prev * current;
                            break;
                        case '÷':
                            if (current === 0) {
                                throw new Error("Division by zero");
                            }
                            computation = prev / current;
                            break;
                        case 'x^y':
                            computation = Math.pow(prev, current);
                            break;
                        default:
                            return;
                    }
                    
                    if (!isFinite(computation)) {
                        throw new Error("Result too large");
                    }
                    
                    this.currentOperand = this.formatResult(computation);
                    this.operation = undefined;
                    this.previousOperand = '';
                    this.shouldResetScreen = true;
                } catch (error) {
                    this.currentOperand = 'Error';
                    this.hasError = true;
                    this.operation = undefined;
                    this.previousOperand = '';
                }
            }
            
            formatResult(num) {
                if (Number.isInteger(num)) {
                    return num.toString();
                } else {
                    return parseFloat(num.toPrecision(12)).toString();
                }
            }
            
            scientificOperation(operation) {
                if (this.hasError) {
                    this.clear();
                }
                
                const current = parseFloat(this.currentOperand);
                
                if (isNaN(current)) return;
                
                try {
                    switch (operation) {
                        case 'x²':
                            this.currentOperand = this.formatResult(current * current);
                            break;
                        case '√':
                            if (current < 0) {
                                throw new Error("Invalid input");
                            }
                            this.currentOperand = this.formatResult(Math.sqrt(current));
                            break;
                        case '1/x':
                            if (current === 0) {
                                throw new Error("Division by zero");
                            }
                            this.currentOperand = this.formatResult(1 / current);
                            break;
                        case '±':
                            this.currentOperand = this.formatResult(-current);
                            break;
                        case 'log':
                            if (current <= 0) {
                                throw new Error("Invalid input");
                            }
                            this.currentOperand = this.formatResult(Math.log10(current));
                            break;
                        case 'ln':
                            if (current <= 0) {
                                throw new Error("Invalid input");
                            }
                            this.currentOperand = this.formatResult(Math.log(current));
                            break;
                        case '10^x':
                            this.currentOperand = this.formatResult(Math.pow(10, current));
                            break;
                        case 'sin':
                            this.currentOperand = this.formatResult(Math.sin(current * Math.PI / 180));
                            break;
                        case 'cos':
                            this.currentOperand = this.formatResult(Math.cos(current * Math.PI / 180));
                            break;
                        case 'tan':
                            this.currentOperand = this.formatResult(Math.tan(current * Math.PI / 180));
                            break;
                        case 'π':
                            this.currentOperand = Math.PI.toString();
                            break;
                        case 'e':
                            this.currentOperand = Math.E.toString();
                            break;
                        case '%':
                            this.currentOperand = this.formatResult(current / 100);
                            break;
                        case 'mc':
                            this.memory = 0;
                            break;
                        case 'mr':
                            this.currentOperand = this.memory.toString();
                            break;
                        case 'm+':
                            this.memory += parseFloat(this.currentOperand);
                            break;
                        case 'm-':
                            this.memory -= parseFloat(this.currentOperand);
                            break;
                        default:
                            return;
                    }
                    
                    this.shouldResetScreen = true;
                } catch (error) {
                    this.currentOperand = 'Error';
                    this.hasError = true;
                }
            }
            
            toggleMode() {
                this.isScientificMode = !this.isScientificMode;
                return this.isScientificMode;
            }
            
            updateDisplay() {
                this.currentOperandElement.innerText = this.currentOperand;
                
                if (this.hasError) {
                    this.currentOperandElement.classList.add('error');
                } else {
                    this.currentOperandElement.classList.remove('error');
                }
                
                if (this.operation != null) {
                    this.previousOperandElement.innerText = 
                        `${this.previousOperand} ${this.operation}`;
                } else {
                    this.previousOperandElement.innerText = '';
                }
            }
        }
        
        // Mobile-optimized firework effect
        function createFirework(event) {
            if (window.fireworkCooldown) return;
            window.fireworkCooldown = true;
            setTimeout(() => { window.fireworkCooldown = false; }, 400);
            
            const display = event.currentTarget;
            const rect = display.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;
            
            const particleCount = 30;
            
            for (let i = 0; i < particleCount; i++) {
                createParticle(centerX, centerY);
            }
            
            display.style.backgroundColor = '#ffeb3b';
            setTimeout(() => {
                display.style.backgroundColor = '#f8f9fa';
            }, 200);
        }
        
        function createParticle(x, y) {
            const particle = document.createElement('div');
            particle.className = 'particle';
            
            const colors = ['#ff0000', '#ff9900', '#ffff00', '#00ff00', '#00ffff', '#0000ff', '#ff00ff'];
            const color = colors[Math.floor(Math.random() * colors.length)];
            particle.style.backgroundColor = color;
            
            const size = Math.random() * 5 + 3;
            particle.style.width = `${size}px`;
            particle.style.height = `${size}px`;
            
            const angle = Math.random() * Math.PI * 2;
            const distance = Math.random() * 100 + 50;
            const deltaX = Math.cos(angle) * distance;
            const deltaY = Math.sin(angle) * distance;
            
            particle.style.left = `${x}px`;
            particle.style.top = `${y}px`;
            
            document.body.appendChild(particle);
            
            const animation = particle.animate([
                { 
                    transform: 'translate(0, 0) scale(1)',
                    opacity: 1
                },
                { 
                    transform: `translate(${deltaX}px, ${deltaY}px) scale(0)`,
                    opacity: 0
                }
            ], {
                duration: Math.random() * 800 + 500,
                easing: 'cubic-bezier(0, .9, .57, 1)'
            });
            
            animation.onfinish = () => {
                if (particle.parentNode) {
                    particle.remove();
                }
            };
        }
        
        // Initialize calculator
        const previousOperandElement = document.querySelector('.previous-operand');
        const currentOperandElement = document.querySelector('.current-operand');
        const calculator = new Calculator(previousOperandElement, currentOperandElement);
        const buttonsGrid = document.getElementById('buttons-grid');
        const display = document.getElementById('display');
        
        // Add touch events
        display.addEventListener('click', createFirework);
        display.addEventListener('touchstart', function(e) {
            e.preventDefault();
            createFirework(e);
        }, { passive: false });
        
        // Button event handlers
        function setupButtonEvents() {
            // Number buttons
            document.querySelectorAll('[data-number]').forEach(button => {
                button.addEventListener('click', () => {
                    calculator.appendNumber(button.innerText);
                    calculator.updateDisplay();
                });
            });
            
            // Operation buttons
            document.querySelectorAll('[data-operation]').forEach(button => {
                button.addEventListener('click', () => {
                    calculator.chooseOperation(button.innerText);
                    calculator.updateDisplay();
                });
            });
            
            // Scientific operation buttons
            document.querySelectorAll('.scientific[data-operation]').forEach(button => {
                button.addEventListener('click', () => {
                    calculator.scientificOperation(button.getAttribute('data-operation'));
                    calculator.updateDisplay();
                });
            });
            
            // Action buttons
            document.querySelectorAll('[data-action]').forEach(button => {
                button.addEventListener('click', () => {
                    const action = button.getAttribute('data-action');
                    
                    if (action === 'calculate') {
                        calculator.calculate();
                    } else if (action === 'clear') {
                        calculator.clear();
                    } else if (action === 'clear-entry') {
                        calculator.clearEntry();
                    } else if (action === 'backspace') {
                        calculator.delete();
                    }
                    
                    calculator.updateDisplay();
                });
            });
        }
        
        setupButtonEvents();
        
        // Mode toggle
        const modeToggle = document.getElementById('mode-toggle');
        modeToggle.addEventListener('click', () => {
            const isScientific = calculator.toggleMode();
            const scientificButtons = document.querySelectorAll('.scientific');
            
            if (isScientific) {
                modeToggle.innerText = 'Switch to Basic Mode';
                buttonsGrid.classList.remove('basic-grid');
                scientificButtons.forEach(button => {
                    button.style.display = 'flex';
                });
            } else {
                modeToggle.innerText = 'Switch to Scientific Mode';
                buttonsGrid.classList.add('basic-grid');
                scientificButtons.forEach(button => {
                    button.style.display = 'none';
                });
            }
        });
        
        // Prevent zoom and other mobile browser behaviors
        document.addEventListener('touchstart', function(e) {
            if (e.touches.length > 1) {
                e.preventDefault();
            }
        }, { passive: false });
        
        let lastTouchEnd = 0;
        document.addEventListener('touchend', function(e) {
            const now = (new Date()).getTime();
            if (now - lastTouchEnd <= 300) {
                e.preventDefault();
            }
            lastTouchEnd = now;
        }, false);
        
        document.addEventListener('gesturestart', function(e) {
            e.preventDefault();
        });
        
        // Keyboard support
        document.addEventListener('keydown', (event) => {
            if (event.key >= '0' && event.key <= '9') {
                calculator.appendNumber(event.key);
                calculator.updateDisplay();
            } else if (event.key === '.') {
                calculator.appendNumber('.');
                calculator.updateDisplay();
            } else if (event.key === '+' || event.key === '-' || event.key === '*' || event.key === '/') {
                let operation;
                switch (event.key) {
                    case '+': operation = '+'; break;
                    case '-': operation = '-'; break;
                    case '*': operation = '×'; break;
                    case '/': operation = '÷'; break;
                }
                calculator.chooseOperation(operation);
                calculator.updateDisplay();
            } else if (event.key === 'Enter' || event.key === '=') {
                calculator.calculate();
                calculator.updateDisplay();
            } else if (event.key === 'Escape') {
                calculator.clear();
                calculator.updateDisplay();
            } else if (event.key === 'Backspace') {
                calculator.delete();
                calculator.updateDisplay();
            }
        });
        
        // Force focus for keyboard input
        document.body.addEventListener('click', function() {
            document.body.focus();
        });
        
        document.body.tabIndex = 0;
        document.body.focus();
    </script>
</body>
</html>
