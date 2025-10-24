<!DOCTYPE html>
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
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: #000000;
            padding: 15px;
            overflow: hidden;
            touch-action: manipulation;
        }
        
        .calculator {
            background-color: #000000;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(255, 255, 255, 0.1);
            width: 100%;
            max-width: 400px;
            overflow: hidden;
            padding: 20px;
            border: 1px solid #333;
            position: relative;
            z-index: 1;
        }
        
        .calculator-inner {
            background-color: #fff;
            border-radius: 16px;
            padding: 18px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .header {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 12px;
            padding-bottom: 8px;
            border-bottom: 1px solid #eaeaea;
        }
        
        .logo {
            height: 35px;
            max-width: 100%;
        }
        
        .display {
            background-color: #f8f9fa;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 18px;
            text-align: right;
            color: #333;
            min-height: 90px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
            border: 1px solid #e9ecef;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            user-select: none;
            -webkit-user-select: none;
        }
        
        .display:hover {
            background-color: #f0f3f5;
        }
        
        .display:active {
            transform: scale(0.98);
        }
        
        .previous-operand {
            font-size: 0.85rem;
            color: #666;
            min-height: 1.1rem;
            word-wrap: break-word;
            word-break: break-all;
        }
        
        .current-operand {
            font-size: 1.8rem;
            font-weight: 500;
            word-wrap: break-word;
            word-break: break-all;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-gap: 8px;
        }
        
        button {
            border: none;
            border-radius: 8px;
            padding: 16px 0;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.1s ease;
            color: #333;
            background-color: #f0f0f0;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            font-weight: 500;
            touch-action: manipulation;
            min-height: 50px;
            user-select: none;
            -webkit-user-select: none;
        }
        
        button:hover {
            background-color: #e0e0e0;
        }
        
        button:active {
            transform: scale(0.95);
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
        }
        
        .operation {
            background-color: #e9ecef;
            color: #495057;
        }
        
        .operation:hover {
            background-color: #dee2e6;
        }
        
        .equals {
            background-color: #007bff;
            color: white;
        }
        
        .equals:hover {
            background-color: #0069d9;
        }
        
        .scientific {
            background-color: #f8f9fa;
            font-size: 0.8rem;
            padding: 14px 0;
        }
        
        .scientific:hover {
            background-color: #e9ecef;
        }
        
        .clear {
            background-color: #f8f9fa;
            color: #dc3545;
        }
        
        .clear:hover {
            background-color: #f1f3f4;
        }
        
        .zero {
            grid-column: span 2;
        }
        
        .mode-toggle {
            background-color: #333;
            color: white;
            font-size: 0.9rem;
            margin-top: 15px;
            width: 100%;
            padding: 12px;
            border-radius: 8px;
            transition: all 0.3s ease;
            border: none;
            min-height: 44px;
        }
        
        .mode-toggle:hover {
            background-color: #555;
        }
        
        .mode-toggle:active {
            transform: scale(0.98);
        }
        
        .footer {
            text-align: center;
            margin-top: 15px;
            padding-top: 10px;
            color: #fff;
            font-size: 0.85rem;
            border-top: 1px solid #333;
        }
        
        .error {
            color: #dc3545;
        }
        
        /* Firework particles */
        .particle {
            position: absolute;
            width: 5px;
            height: 5px;
            border-radius: 50%;
            pointer-events: none;
            z-index: 1000;
        }
        
        /* Mobile-specific optimizations */
        @media (max-width: 480px) {
            .calculator {
                padding: 15px;
                border-radius: 18px;
            }
            
            .calculator-inner {
                padding: 15px;
                border-radius: 14px;
            }
            
            .display {
                padding: 12px;
                min-height: 80px;
                margin-bottom: 15px;
            }
            
            .current-operand {
                font-size: 1.6rem;
            }
            
            .previous-operand {
                font-size: 0.8rem;
            }
            
            button {
                padding: 14px 0;
                font-size: 0.9rem;
                min-height: 45px;
            }
            
            .scientific {
                font-size: 0.75rem;
                padding: 12px 0;
            }
            
            .buttons-grid {
                grid-gap: 6px;
            }
            
            .logo {
                height: 30px;
            }
        }
        
        @media (max-width: 360px) {
            .calculator {
                padding: 12px;
            }
            
            .calculator-inner {
                padding: 12px;
            }
            
            .display {
                padding: 10px;
                min-height: 70px;
            }
            
            .current-operand {
                font-size: 1.4rem;
            }
            
            button {
                padding: 12px 0;
                font-size: 0.85rem;
                min-height: 40px;
            }
            
            .scientific {
                font-size: 0.7rem;
                padding: 10px 0;
            }
            
            .buttons-grid {
                grid-gap: 5px;
            }
        }
        
        /* Prevent text selection on buttons */
        .no-select {
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            -khtml-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="calculator-inner">
            <div class="header">
                <img src="https://i.ibb.co/dsTTy0dd/image.png" alt="Calculator Logo" class="logo">
            </div>
            
            <div class="display" id="display">
                <div class="previous-operand"></div>
                <div class="current-operand">0</div>
            </div>
            
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
            <button class="mode-toggle no-select" id="mode-toggle">Switch to Basic Mode</button>
        </div>
        <div class="footer">
            Made in 💖 By Armeen
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
                    
                    // Handle very large or very small numbers
                    if (!isFinite(computation)) {
                        throw new Error("Result too large");
                    }
                    
                    // Format the result to avoid floating point precision issues
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
                // Format numbers to avoid long decimal strings
                if (Number.isInteger(num)) {
                    return num.toString();
                } else {
                    // Limit to 10 decimal places
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
            // Prevent multiple rapid triggers on mobile
            if (window.fireworkCooldown) return;
            window.fireworkCooldown = true;
            setTimeout(() => { window.fireworkCooldown = false; }, 300);
            
            const display = event.currentTarget;
            const rect = display.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;
            
            // Create fewer particles on mobile for better performance
            const particleCount = window.innerWidth < 480 ? 25 : 40;
            
            for (let i = 0; i < particleCount; i++) {
                createParticle(centerX, centerY);
            }
            
            // Add a subtle flash effect to the display
            display.style.backgroundColor = '#ffeb3b';
            setTimeout(() => {
                display.style.backgroundColor = '#f8f9fa';
            }, 150);
        }
        
        function createParticle(x, y) {
            const particle = document.createElement('div');
            particle.className = 'particle';
            
            // Random colors for the particles
            const colors = ['#ff0000', '#ff9900', '#ffff00', '#00ff00', '#00ffff', '#0000ff', '#ff00ff'];
            const color = colors[Math.floor(Math.random() * colors.length)];
            particle.style.backgroundColor = color;
            
            // Random size (smaller on mobile)
            const size = Math.random() * 4 + 2;
            particle.style.width = `${size}px`;
            particle.style.height = `${size}px`;
            
            // Random position around the click point
            const angle = Math.random() * Math.PI * 2;
            const distance = Math.random() * 80 + 40;
            const deltaX = Math.cos(angle) * distance;
            const deltaY = Math.sin(angle) * distance;
            
            // Set initial position
            particle.style.left = `${x}px`;
            particle.style.top = `${y}px`;
            
            document.body.appendChild(particle);
            
            // Shorter animation on mobile
            const duration = window.innerWidth < 480 ? 600 : 800;
            
            // Animate the particle
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
                duration: Math.random() * duration + 400,
                easing: 'cubic-bezier(0, .9, .57, 1)'
            });
            
            // Remove particle after animation completes
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
        
        // Add touch event for mobile firework effect
        display.addEventListener('click', createFirework);
        display.addEventListener('touchstart', function(e) {
            e.preventDefault(); // Prevent double-tap zoom
            createFirework(e);
        }, { passive: false });
        
        // Number buttons
        document.querySelectorAll('[data-number]').forEach(button => {
            button.addEventListener('click', () => {
                calculator.appendNumber(button.innerText);
                calculator.updateDisplay();
            });
            
            // Add touch events for mobile
            button.addEventListener('touchstart', function(e) {
                e.preventDefault();
                this.style.transform = 'scale(0.95)';
            });
            
            button.addEventListener('touchend', function(e) {
                e.preventDefault();
                this.style.transform = '';
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
            
            // Add touch events for mobile
            button.addEventListener('touchstart', function(e) {
                e.preventDefault();
                this.style.transform = 'scale(0.95)';
            });
            
            button.addEventListener('touchend', function(e) {
                e.preventDefault();
                this.style.transform = '';
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
            
            // Add touch events for mobile
            button.addEventListener('touchstart', function(e) {
                e.preventDefault();
                this.style.transform = 'scale(0.95)';
            });
            
            button.addEventListener('touchend', function(e) {
                e.preventDefault();
                this.style.transform = '';
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
            
            // Add touch events for mobile
            button.addEventListener('touchstart', function(e) {
                e.preventDefault();
                this.style.transform = 'scale(0.95)';
            });
            
            button.addEventListener('touchend', function(e) {
                e.preventDefault();
                this.style.transform = '';
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
        
        // Mode toggle
        const modeToggle = document.getElementById('mode-toggle');
        modeToggle.addEventListener('click', () => {
            const isScientific = calculator.toggleMode();
            const scientificButtons = document.querySelectorAll('.scientific');
            
            if (isScientific) {
                modeToggle.innerText = 'Switch to Basic Mode';
                buttonsGrid.classList.remove('basic-grid');
                scientificButtons.forEach(button => {
                    button.style.display = 'block';
                });
            } else {
                modeToggle.innerText = 'Switch to Scientific Mode';
                buttonsGrid.classList.add('basic-grid');
                scientificButtons.forEach(button => {
                    button.style.display = 'none';
                });
            }
        });
        
        // Add touch events for mode toggle
        modeToggle.addEventListener('touchstart', function(e) {
            e.preventDefault();
            this.style.transform = 'scale(0.98)';
        });
        
        modeToggle.addEventListener('touchend', function(e) {
            e.preventDefault();
            this.style.transform = '';
            const isScientific = calculator.toggleMode();
            const scientificButtons = document.querySelectorAll('.scientific');
            
            if (isScientific) {
                modeToggle.innerText = 'Switch to Basic Mode';
                buttonsGrid.classList.remove('basic-grid');
                scientificButtons.forEach(button => {
                    button.style.display = 'block';
                });
            } else {
                modeToggle.innerText = 'Switch to Scientific Mode';
                buttonsGrid.classList.add('basic-grid');
                scientificButtons.forEach(button => {
                    button.style.display = 'none';
                });
            }
        });
        
        // Prevent zoom on double tap
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
        
        // Keyboard support (for devices with physical keyboards)
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
    </script>
</body>
</html>
