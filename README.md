<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Scruentic Calculator</title>
<style>
  :root{
    --bg:#0f1724; --panel:#0b1220; --accent:#7c3aed; --txt:#e6eef8;
    --btn:#131a27; --btn-alt:#162133;
    font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
  }
  *{box-sizing:border-box}
  body{margin:0;display:flex;min-height:100vh;align-items:center;justify-content:center;background:
    linear-gradient(180deg,#061022 0%, #071226 100%);color:var(--txt)}
  .calc{
    width:360px;border-radius:14px;padding:18px;background:linear-gradient(180deg,var(--panel),#071226);
    box-shadow: 0 10px 30px rgba(2,6,23,0.6);
  }
  .display{
    background:linear-gradient(180deg,#071226,#091627);
    border-radius:10px;padding:12px;color:var(--txt);
    min-height:74px;margin-bottom:12px;display:flex;flex-direction:column;justify-content:center;
  }
  .small {
    font-size:13px;opacity:0.75;height:18px;overflow:hidden;
  }
  .expr{font-size:20px;word-break:break-all}
  .row{display:grid;grid-template-columns:repeat(6,1fr);gap:8px;margin-bottom:8px}
  button{
    background:var(--btn);border:0;padding:12px;border-radius:8px;color:var(--txt);
    font-size:16px;cursor:pointer;box-shadow:inset 0 -2px rgba(0,0,0,0.2);
  }
  button.fw{grid-column:span 2}
  button.op{background:linear-gradient(180deg,var(--btn-alt),#0c1726)}
  button.acc{background:linear-gradient(180deg,var(--accent),#5b21b6);font-weight:600}
  .mode{display:flex;gap:8px;align-items:center;margin-bottom:10px}
  .mode button{padding:6px 10px;font-size:13px;border-radius:999px}
  .footer{display:flex;justify-content:space-between;font-size:12px;opacity:0.9;margin-top:8px}
  .history{max-height:140px;overflow:auto;margin-top:6px;font-size:13px;opacity:0.9}
  .mem{margin-left:8px;padding:4px 8px;border-radius:6px;background:#081226}
</style>
</head>
<body>
  <div class="calc" role="application" aria-label="Scientific calculator">
    <div class="display" id="display">
      <div class="small" id="modeLabel">RAD</div>
      <div class="expr" id="expr">0</div>
    </div>

    <div class="mode">
      <button id="degRad" title="toggle degrees / radians">RAD ↔ DEG</button>
      <div style="flex:1"></div>
      <button id="mc">MC</button>
      <button id="mr">MR</button>
      <button id="mPlus">M+</button>
      <button id="mMinus">M-</button>
    </div>

    <div class="row">
      <button class="op" data-val="(">(</button>
      <button class="op" data-val=")">)</button>
      <button data-val="%" class="op">%</button>
      <button data-val="C" id="clear">C</button>
      <button data-val="⌫" id="back">⌫</button>
      <button data-val="/" class="op">÷</button>

      <button data-val="7">7</button>
      <button data-val="8">8</button>
      <button data-val="9">9</button>
      <button data-val="*" class="op">×</button>
      <button data-val="^" class="op">^</button>
      <button data-val="sqrt" class="op">√</button>

      <button data-val="4">4</button>
      <button data-val="5">5</button>
      <button data-val="6">6</button>
      <button data-val="-" class="op">−</button>
      <button data-val="!" class="op">n!</button>
      <button data-val="%" class="op">mod</button>

      <button data-val="1">1</button>
      <button data-val="2">2</button>
      <button data-val="3">3</button>
      <button data-val="+" class="op">+</button>
      <button data-val="pi" class="op">π</button>
      <button data-val="e" class="op">e</button>

      <button data-val="0" class="fw">0</button>
      <button data-val=".">.</button>
      <button id="ans" class="op">Ans</button>
      <button id="equals" class="acc" style="grid-column:span 2">=</button>
    </div>

    <div class="row" style="margin-top:6px">
      <button data-val="sin">sin</button>
      <button data-val="cos">cos</button>
      <button data-val="tan">tan</button>
      <button data-val="asin">asin</button>
      <button data-val="acos">acos</button>
      <button data-val="atan">atan</button>

      <button data-val="ln">ln</button>
      <button data-val="log">log</button>
      <button data-val="exp">exp</button>
      <button data-val="abs">abs</button>
      <button data-val="pow">pow(x,y)</button>
      <button data-val="mod" class="op">mod</button>
    </div>

    <div class="history" id="history" aria-live="polite"></div>

    <div class="footer">
      <div>Memory: <span id="memVal">0</span></div>
      <div><span class="mem" id="lastAns">Ans: 0</span></div>
    </div>
  </div>

<script>
(() => {
  const exprEl = document.getElementById('expr');
  const modeLabel = document.getElementById('modeLabel');
  const historyEl = document.getElementById('history');
  const memValEl = document.getElementById('memVal');
  const lastAnsEl = document.getElementById('lastAns');

  let expression = '';
  let memory = 0;
  let lastAns = 0;
  let isDeg = false;

  function updateDisplay(){
    exprEl.textContent = expression || '0';
    modeLabel.textContent = isDeg ? 'DEG' : 'RAD';
    memValEl.textContent = memory;
    lastAnsEl.textContent = 'Ans: ' + (String(lastAns));
  }

  function push(val){
    // convenience: function names or symbols appended
    if(val === 'C'){ expression = ''; updateDisplay(); return; }
    if(val === '⌫'){ expression = expression.slice(0,-1); updateDisplay(); return; }
    if(val === 'Ans'){ expression += (lastAns === null ? '' : String(lastAns)); updateDisplay(); return; }
    expression += val;
    updateDisplay();
  }

  // factorial replacement helper supporting (expr)! recursively
  function replaceFactorial(expr){
    // handle nested parenthesis factorials first: ( ... )!
    const parenthesisFactorial = /\(([^()]+)\)!/;
    while(parenthesisFactorial.test(expr)){
      expr = expr.replace(parenthesisFactorial, (_m, g1) => `factorial(${g1})`);
    }
    // then simple number or variable like 5! or x!
    expr = expr.replace(/(\d+(\.\d+)?|[A-Za-z_][A-Za-z0-9_.]*)!/g, 'factorial($1)');
    return expr;
  }

  // safe evaluate (local math functions provided)
  function evaluate(expr){
    // normalize common symbols
    expr = expr.replace(/÷/g, '/').replace(/×/g, '*').replace(/−/g, '-').replace(/π/g, 'pi').replace(/×/g,'*');
    // replace ^ with ** for power
    expr = expr.replace(/\^/g, '**');
    // replace pow(x,y) -> (x**y) OR keep pow and map below
    expr = expr.replace(/pow\(/g, 'pow(');
    // handle factorial
    expr = replaceFactorial(expr);

    // percent handling: e.g., "50%" -> "(50/100)"
    expr = expr.replace(/(\d+(\.\d+)?)%/g, '($1/100)');

    // Allowed names; we'll provide them as function args to Function
    // Provide trig wrappers to handle DEG/RAD
    const degFactor = isDeg ? (v => v * Math.PI / 180) : (v => v);
    const sin = (x)=> Math.sin(degFactor(x));
    const cos = (x)=> Math.cos(degFactor(x));
    const tan = (x)=> Math.tan(degFactor(x));
    const asin = (x)=> isDeg ? (Math.asin(x)*180/Math.PI) : Math.asin(x);
    const acos = (x)=> isDeg ? (Math.acos(x)*180/Math.PI) : Math.acos(x);
    const atan = (x)=> isDeg ? (Math.atan(x)*180/Math.PI) : Math.atan(x);
    const ln = (x)=> Math.log(x);
    const log = (x)=> Math.log10 ? Math.log10(x) : Math.log(x)/Math.LN10;
    const exp = (x)=> Math.exp(x);
    const sqrt = (x)=> Math.sqrt(x);
    const abs = (x)=> Math.abs(x);
    const pow = (a,b)=> Math.pow(a,b);
    const pi = Math.PI;
    const e = Math.E;
    const mod = (a,b)=> a % b;

    function factorial(n){
      n = Number(n);
      if(!isFinite(n) || n < 0) throw 'factorial domain';
      if(n > 170) throw 'factorial overflow';
      if(Math.floor(n) !== n) throw 'factorial only for integers';
      let f = 1;
      for(let i=2;i<=n;i++) f*=i;
      return f;
    }

    // Provide Ans variable
    const Ans = lastAns;

    // Restrict characters: allow digits, letters, operators, parentheses, dot, comma, underscore
    // (we'll still rely on Function generation; this app runs locally)
    // Build function that returns the evaluated value using the provided helpers
    try {
      const func = new Function(
        'sin','cos','tan','asin','acos','atan','ln','log','exp','sqrt','abs','pow','pi','e','factorial','mod','Ans',
        '"use strict"; return (' + expr + ');'
      );
      const result = func(sin,cos,tan,asin,acos,atan,ln,log,exp,sqrt,abs,pow,pi,e,factorial,mod,Ans);
      if(typeof result === 'number' && !isFinite(result)) throw 'Result not finite';
      return result;
    } catch (err){
      throw err;
    }
  }

  // Buttons
  document.querySelectorAll('button[data-val]').forEach(b => {
    b.addEventListener('click', ()=> {
      const v = b.getAttribute('data-val');
      // map some labels to JS-friendly tokens
      if(v === 'sqrt') push('sqrt(');
      else if(v === 'ln') push('ln(');
      else if(v === 'log') push('log(');
      else if(v === 'sin' || v === 'cos' || v === 'tan' || v === 'asin' || v === 'acos' || v === 'atan')
        push(v + '(');
      else if(v === 'exp') push('exp(');
      else if(v === 'pow') push('pow(');
      else if(v === 'pi') push('pi');
      else if(v === 'e') push('e');
      else if(v === '!') push('!');
      else if(v === 'mod') push('%'); // mapped mod symbolically; mod function also available
      else push(v);
    });
  });

  document.getElementById('clear').addEventListener('click', ()=> { expression=''; updateDisplay(); });
  document.getElementById('back').addEventListener('click', ()=> { expression = expression.slice(0,-1); updateDisplay(); });

  document.getElementById('equals').addEventListener('click', doEvaluate);
  document.getElementById('degRad').addEventListener('click', ()=> { isDeg = !isDeg; updateDisplay(); });

  // Memory controls
  document.getElementById('mc').addEventListener('click', ()=> { memory = 0; updateDisplay(); });
  document.getElementById('mr').addEventListener('click', ()=> { expression += String(memory); updateDisplay(); });
  document.getElementById('mPlus').addEventListener('click', ()=> {
    try { memory = Number(memory) + Number(evaluate(expression || String(lastAns))); updateDisplay(); }
    catch(e){ alert('Eval error: ' + e); }
  });
  document.getElementById('mMinus').addEventListener('click', ()=> {
    try { memory = Number(memory) - Number(evaluate(expression || String(lastAns))); updateDisplay(); }
    catch(e){ alert('Eval error: ' + e); }
  });

  document.getElementById('ans').addEventListener('click', ()=> { expression += String(lastAns); updateDisplay(); });

  function addHistory(input, output){
    const item = document.createElement('div');
    item.textContent = input + ' = ' + output;
    historyEl.prepend(item);
  }

  function doEvaluate(){
    if(!expression) return;
    try {
      const result = evaluate(expression);
      addHistory(expression, result);
      lastAns = result;
      expression = String(result);
      updateDisplay();
    } catch (err) {
      alert('Error: ' + err);
    }
  }

  // Keyboard support
  window.addEventListener('keydown', (ev)=>{
    const k = ev.key;
    if((/^[0-9.]$/).test(k)) { push(k); ev.preventDefault(); return; }
    if(k === 'Enter' || k === '='){ doEvaluate(); ev.preventDefault(); return; }
    if(k === 'Backspace'){ expression = expression.slice(0,-1); updateDisplay(); ev.preventDefault(); return; }
    if(k === 'Escape'){ expression = ''; updateDisplay(); ev.preventDefault(); return; }
    if(k === '+'||k === '-'||k === '*'||k === '/'||k === '^') { push(k); ev.preventDefault(); return; }
    if(k === '('||k===')') { push(k); ev.preventDefault(); return; }
    // allow common function shortcuts: s->sin(, c->cos(, t->tan(, l->ln( or log)
    if(k.toLowerCase()==='s'){ push('sin('); ev.preventDefault(); return; }
    if(k.toLowerCase()==='c'){ push('cos('); ev.preventDefault(); return; }
    if(k.toLowerCase()==='t'){ push('tan('); ev.preventDefault(); return; }
    if(k.toLowerCase()==='l'){ push('ln('); ev.preventDefault(); return; }
  });

  // init
  expression = '';
  updateDisplay();
})();
</script>
</body>
</html>
