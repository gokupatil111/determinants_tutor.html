<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Determinants — Interactive Tutor</title>
<link href="https://fonts.googleapis.com/css2?family=Baloo+2:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #0f0e17;
  --bg2: #1a1828;
  --bg3: #231f35;
  --card: #1e1b2e;
  --border: rgba(255,255,255,0.08);
  --purple: #a78bfa;
  --purple2: #7c3aed;
  --pink: #f472b6;
  --teal: #2dd4bf;
  --amber: #fbbf24;
  --green: #4ade80;
  --red: #f87171;
  --text: #e8e6f0;
  --muted: #8b7fb8;
  --font: 'Baloo 2', sans-serif;
  --mono: 'JetBrains Mono', monospace;
}
*{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth}
body{background:var(--bg);color:var(--text);font-family:var(--font);min-height:100vh;overflow-x:hidden}

/* Stars bg */
body::before{content:'';position:fixed;inset:0;background:radial-gradient(ellipse at 20% 50%,rgba(124,58,237,0.08) 0%,transparent 60%),radial-gradient(ellipse at 80% 20%,rgba(244,114,182,0.06) 0%,transparent 50%);pointer-events:none;z-index:0}

/* NAV */
nav{position:sticky;top:0;z-index:100;background:rgba(15,14,23,0.85);backdrop-filter:blur(12px);border-bottom:1px solid var(--border);display:flex;align-items:center;gap:4px;padding:0 1rem;overflow-x:auto}
.nav-btn{padding:14px 16px;font-family:var(--font);font-size:13px;font-weight:600;color:var(--muted);background:none;border:none;cursor:pointer;white-space:nowrap;border-bottom:2px solid transparent;transition:all 0.2s}
.nav-btn:hover{color:var(--text)}
.nav-btn.active{color:var(--purple);border-bottom-color:var(--purple)}
.nav-logo{font-size:16px;font-weight:800;color:var(--purple);margin-right:12px;white-space:nowrap}

/* PAGES */
.page{display:none;padding:2rem 1rem;max-width:800px;margin:0 auto;position:relative;z-index:1;animation:fadeIn 0.3s ease}
.page.active{display:block}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}

/* HERO */
.hero{text-align:center;padding:3rem 1rem 2rem}
.hero h1{font-size:clamp(2rem,6vw,3.5rem);font-weight:800;line-height:1.1;margin-bottom:1rem}
.hero h1 span{background:linear-gradient(135deg,var(--purple),var(--pink));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.hero p{color:var(--muted);font-size:16px;max-width:500px;margin:0 auto 2rem;line-height:1.7}
.hero-cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:12px;margin-top:2rem}
.hero-card{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:1.25rem;cursor:pointer;transition:all 0.2s;text-align:left}
.hero-card:hover{border-color:var(--purple);transform:translateY(-2px);background:var(--bg3)}
.hero-card .icon{font-size:24px;margin-bottom:8px}
.hero-card .title{font-size:14px;font-weight:700;margin-bottom:4px}
.hero-card .desc{font-size:12px;color:var(--muted)}

/* SECTION HEADERS */
.sec-head{margin-bottom:1.5rem}
.sec-head h2{font-size:clamp(1.4rem,4vw,2rem);font-weight:800;margin-bottom:0.5rem}
.sec-head h2 span{color:var(--purple)}
.sec-head p{color:var(--muted);font-size:14px;line-height:1.6}

/* CARDS */
.card{background:var(--card);border:1px solid var(--border);border-radius:16px;padding:1.5rem;margin-bottom:1rem}
.card-title{font-size:16px;font-weight:700;margin-bottom:0.75rem;display:flex;align-items:center;gap:8px}
.card-title .dot{width:8px;height:8px;border-radius:50%;background:var(--purple);flex-shrink:0}

/* MATRIX */
.matrix-wrap{display:inline-flex;align-items:center;gap:4px;margin:8px 0}
.matrix-bracket{font-size:36px;color:var(--muted);line-height:1;font-weight:200}
.matrix-grid{display:grid;gap:6px;padding:4px 8px}
.mcell{width:38px;height:38px;display:flex;align-items:center;justify-content:center;font-size:15px;font-weight:600;border-radius:8px;background:var(--bg3);color:var(--text);font-family:var(--mono);border:1px solid var(--border)}
.mcell.hi-p{background:rgba(167,139,250,0.2);border-color:var(--purple);color:var(--purple)}
.mcell.hi-b{background:rgba(244,114,182,0.2);border-color:var(--pink);color:var(--pink)}
.mcell.hi-g{background:rgba(74,222,128,0.2);border-color:var(--green);color:var(--green)}
.mcell.faded{opacity:0.25}
.mcell.strike{background:rgba(248,113,113,0.1);border-color:var(--red);color:var(--red);text-decoration:line-through}

/* FORMULA BOX */
.formula{background:rgba(124,58,237,0.12);border:1px solid rgba(167,139,250,0.3);border-radius:10px;padding:12px 16px;font-family:var(--mono);font-size:14px;color:var(--purple);margin:12px 0;text-align:center;font-weight:600}

/* STEPS */
.steps{margin:12px 0}
.step{display:flex;gap:12px;align-items:flex-start;margin:10px 0;opacity:0;transform:translateX(-10px);transition:all 0.3s}
.step.show{opacity:1;transform:translateX(0)}
.step-num{min-width:28px;height:28px;border-radius:50%;background:var(--purple2);color:white;font-size:12px;font-weight:700;display:flex;align-items:center;justify-content:center;flex-shrink:0;margin-top:2px}
.step-txt{font-size:14px;color:var(--text);line-height:1.7}
.step-txt strong{color:var(--teal)}

/* BTN */
.btn{padding:10px 20px;border-radius:10px;border:1px solid var(--border);background:var(--bg3);cursor:pointer;font-family:var(--font);font-size:14px;font-weight:600;color:var(--text);transition:all 0.2s;display:inline-flex;align-items:center;gap:6px}
.btn:hover{background:var(--purple2);border-color:var(--purple);color:white}
.btn.primary{background:var(--purple2);border-color:var(--purple);color:white}
.btn.primary:hover{background:#6d28d9}
.btn-row{display:flex;gap:8px;flex-wrap:wrap;margin-top:12px}

/* BADGE */
.badge{display:inline-block;padding:3px 10px;border-radius:20px;font-size:11px;font-weight:700;margin-left:8px}
.badge.easy{background:rgba(74,222,128,0.15);color:var(--green);border:1px solid rgba(74,222,128,0.3)}
.badge.med{background:rgba(251,191,36,0.15);color:var(--amber);border:1px solid rgba(251,191,36,0.3)}
.badge.hard{background:rgba(248,113,113,0.15);color:var(--red);border:1px solid rgba(248,113,113,0.3)}

/* GAME STUFF */
.game-area{background:var(--bg2);border:1px solid var(--border);border-radius:16px;padding:1.5rem;margin-top:1rem}
.game-title{font-size:18px;font-weight:800;margin-bottom:0.5rem;display:flex;align-items:center;gap:8px}
.score-bar{display:flex;justify-content:space-between;align-items:center;margin-bottom:1.5rem;padding:12px 16px;background:var(--bg3);border-radius:10px;border:1px solid var(--border)}
.score-item{text-align:center}
.score-item .val{font-size:22px;font-weight:800;color:var(--purple)}
.score-item .lbl{font-size:11px;color:var(--muted);font-weight:600}
.q-box{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:1.25rem;margin-bottom:1rem}
.q-text{font-size:15px;font-weight:600;margin-bottom:1rem;color:var(--text)}
.options{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.opt{padding:12px;border-radius:10px;border:1px solid var(--border);background:var(--bg3);cursor:pointer;font-family:var(--mono);font-size:15px;font-weight:600;color:var(--text);transition:all 0.2s;text-align:center}
.opt:hover:not(.locked){background:rgba(167,139,250,0.15);border-color:var(--purple)}
.opt.correct{background:rgba(74,222,128,0.2);border-color:var(--green);color:var(--green)}
.opt.wrong{background:rgba(248,113,113,0.15);border-color:var(--red);color:var(--red)}
.opt.locked{cursor:default}
.feedback{padding:12px 16px;border-radius:10px;margin-top:10px;font-size:14px;font-weight:600;display:none}
.feedback.show{display:block}
.feedback.ok{background:rgba(74,222,128,0.1);border:1px solid rgba(74,222,128,0.3);color:var(--green)}
.feedback.no{background:rgba(248,113,113,0.1);border:1px solid rgba(248,113,113,0.3);color:var(--red)}

/* CALC GAME */
.calc-input{width:100%;padding:12px 16px;border-radius:10px;border:1px solid var(--border);background:var(--bg3);font-family:var(--mono);font-size:18px;font-weight:700;color:var(--text);text-align:center;outline:none;margin:10px 0}
.calc-input:focus{border-color:var(--purple);box-shadow:0 0 0 3px rgba(167,139,250,0.1)}
.calc-input.correct{border-color:var(--green);background:rgba(74,222,128,0.1);color:var(--green)}
.calc-input.wrong{border-color:var(--red);background:rgba(248,113,113,0.1);color:var(--red)}

/* PROGRESS */
.progress-wrap{background:var(--bg3);border-radius:100px;height:6px;margin:12px 0}
.progress-bar{height:6px;border-radius:100px;background:linear-gradient(90deg,var(--purple),var(--pink));transition:width 0.3s}

/* SIGN GRID */
.sign-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:6px;max-width:160px}
.sign-cell{height:44px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:20px;font-weight:800;border:1px solid var(--border)}
.sign-plus{background:rgba(74,222,128,0.15);color:var(--green);border-color:rgba(74,222,128,0.3)}
.sign-minus{background:rgba(248,113,113,0.15);color:var(--red);border-color:rgba(248,113,113,0.3)}

/* PROPERTY CARD */
.prop-item{border-left:3px solid var(--purple);padding-left:14px;margin:12px 0}
.prop-title{font-size:14px;font-weight:700;color:var(--purple);margin-bottom:4px}
.prop-desc{font-size:13px;color:var(--muted);line-height:1.6}
.prop-example{font-family:var(--mono);font-size:12px;color:var(--teal);margin-top:4px}

/* CRAMER TABLE */
.cramer-table{width:100%;border-collapse:collapse;margin:12px 0;font-size:13px}
.cramer-table th{background:var(--bg3);padding:10px 12px;text-align:left;color:var(--purple);font-size:12px;font-weight:700;border-bottom:1px solid var(--border)}
.cramer-table td{padding:10px 12px;border-bottom:1px solid var(--border);font-family:var(--mono);color:var(--text)}
.cramer-table tr:last-child td{border-bottom:none}

/* MATCH GAME */
.match-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.match-item{padding:12px;border-radius:10px;border:1px solid var(--border);background:var(--bg3);cursor:pointer;font-size:13px;font-weight:600;text-align:center;transition:all 0.2s;min-height:52px;display:flex;align-items:center;justify-content:center}
.match-item:hover:not(.matched):not(.selected){border-color:var(--amber);color:var(--amber)}
.match-item.selected{border-color:var(--purple);background:rgba(167,139,250,0.15);color:var(--purple)}
.match-item.matched{border-color:var(--green);background:rgba(74,222,128,0.1);color:var(--green);cursor:default}
.match-item.wrong-sel{border-color:var(--red);background:rgba(248,113,113,0.1);color:var(--red)}

/* FINISH SCREEN */
.finish{text-align:center;padding:2rem}
.finish .big-score{font-size:4rem;font-weight:800;background:linear-gradient(135deg,var(--purple),var(--pink));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.finish p{color:var(--muted);margin:8px 0 1.5rem;font-size:15px}
.stars{font-size:2rem;margin:8px 0}

/* TOOLTIP */
.tip{background:var(--bg3);border:1px solid var(--border);border-radius:8px;padding:8px 12px;font-size:12px;color:var(--muted);margin-top:8px;line-height:1.5}
.tip strong{color:var(--amber)}

/* RESPONSIVE */
@media(max-width:500px){
  .options{grid-template-columns:1fr}
  .mcell{width:32px;height:32px;font-size:13px}
  .matrix-bracket{font-size:28px}
}
</style>
</head>
<body>

<nav>
  <div class="nav-logo">∣D∣</div>
  <button class="nav-btn active" onclick="goPage('home')">🏠 Home</button>
  <button class="nav-btn" onclick="goPage('det2')">2×2</button>
  <button class="nav-btn" onclick="goPage('det3')">3×3</button>
  <button class="nav-btn" onclick="goPage('props')">Properties</button>
  <button class="nav-btn" onclick="goPage('minor')">Minor & Cofactor</button>
  <button class="nav-btn" onclick="goPage('cramer')">Cramer's Rule</button>
  <button class="nav-btn" onclick="goPage('games')">🎮 Games</button>
</nav>

<!-- HOME -->
<div id="page-home" class="page active">
  <div class="hero">
    <h1>Determinants<br><span>Ekdum Seedha!</span></h1>
    <p>Teri PDF ka poora Chapter 3 — concepts, examples aur games ke saath. Koi bhi topic choose karo!</p>
    <div class="hero-cards">
      <div class="hero-card" onclick="goPage('det2')">
        <div class="icon">2️⃣</div>
        <div class="title">2×2 Determinant</div>
        <div class="desc">ad − bc formula</div>
      </div>
      <div class="hero-card" onclick="goPage('det3')">
        <div class="icon">3️⃣</div>
        <div class="title">3×3 Determinant</div>
        <div class="desc">Row expansion method</div>
      </div>
      <div class="hero-card" onclick="goPage('props')">
        <div class="icon">📋</div>
        <div class="title">Properties</div>
        <div class="desc">5 important rules</div>
      </div>
      <div class="hero-card" onclick="goPage('minor')">
        <div class="icon">✂️</div>
        <div class="title">Minor & Cofactor</div>
        <div class="desc">Row/column delete karo</div>
      </div>
      <div class="hero-card" onclick="goPage('cramer')">
        <div class="icon">🔢</div>
        <div class="title">Cramer's Rule</div>
        <div class="desc">Equations solve karo</div>
      </div>
      <div class="hero-card" onclick="goPage('games')">
        <div class="icon">🎮</div>
        <div class="title">Games</div>
        <div class="desc">Practice karo khelke!</div>
      </div>
    </div>
  </div>
</div>

<!-- 2x2 PAGE -->
<div id="page-det2" class="page">
  <div class="sec-head">
    <h2>2×2 <span>Determinant</span></h2>
    <p>Sabse simple — sirf 4 numbers, 1 formula!</p>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot"></div>Definition kya hai?</div>
    <p style="font-size:14px;color:var(--muted);line-height:1.7;margin-bottom:12px">
      2×2 determinant ek number hota hai jo 4 elements se milke banta hai. Isko vertical bars ke beech likhte hain — jaise |A|
    </p>
    <div style="display:flex;align-items:center;gap:20px;flex-wrap:wrap">
      <div class="matrix-wrap">
        <span class="matrix-bracket">|</span>
        <div class="matrix-grid" style="grid-template-columns:1fr 1fr">
          <div class="mcell hi-p">a</div><div class="mcell hi-b">b</div>
          <div class="mcell hi-b">c</div><div class="mcell hi-p">d</div>
        </div>
        <span class="matrix-bracket">|</span>
      </div>
      <div style="font-size:13px;color:var(--muted);line-height:2">
        🟣 Purple diagonal → <strong style="color:var(--purple)">a × d</strong><br>
        🩷 Pink diagonal → <strong style="color:var(--pink)">b × c</strong><br>
        Answer = (a×d) − (b×c)
      </div>
    </div>
    <div class="formula">|A| = ad − bc</div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--teal)"></div>Example 1 <span class="badge easy">Easy</span></div>
    <div style="margin-bottom:12px;font-size:14px;color:var(--muted)">Find karo:</div>
    <div class="matrix-wrap">
      <span class="matrix-bracket">|</span>
      <div class="matrix-grid" style="grid-template-columns:1fr 1fr">
        <div class="mcell hi-p">7</div><div class="mcell hi-b">9</div>
        <div class="mcell hi-b">−4</div><div class="mcell hi-p">3</div>
      </div>
      <span class="matrix-bracket">|</span>
    </div>
    <div class="steps" id="ex2-steps">
      <div class="step" id="s2-1"><div class="step-num">1</div><div class="step-txt">Purple diagonal: <strong>7 × 3 = 21</strong></div></div>
      <div class="step" id="s2-2"><div class="step-num">2</div><div class="step-txt">Pink diagonal: <strong>9 × (−4) = −36</strong></div></div>
      <div class="step" id="s2-3"><div class="step-num">3</div><div class="step-txt">Answer = 21 − (−36) = 21 + 36 = <strong>57 ✅</strong></div></div>
    </div>
    <div class="btn-row">
      <button class="btn primary" onclick="showSteps2()">Steps dikhao</button>
    </div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--amber)"></div>Example 2 <span class="badge easy">Easy</span></div>
    <div style="margin-bottom:12px;font-size:14px;color:var(--muted)">Find karo:</div>
    <div class="matrix-wrap">
      <span class="matrix-bracket">|</span>
      <div class="matrix-grid" style="grid-template-columns:1fr 1fr">
        <div class="mcell hi-p">5</div><div class="mcell hi-b">3</div>
        <div class="mcell hi-b">7</div><div class="mcell hi-p">9</div>
      </div>
      <span class="matrix-bracket">|</span>
    </div>
    <div class="steps" id="ex2b-steps">
      <div class="step" id="s2b-1"><div class="step-num">1</div><div class="step-txt">Purple diagonal: <strong>5 × 9 = 45</strong></div></div>
      <div class="step" id="s2b-2"><div class="step-num">2</div><div class="step-txt">Pink diagonal: <strong>3 × 7 = 21</strong></div></div>
      <div class="step" id="s2b-3"><div class="step-num">3</div><div class="step-txt">Answer = 45 − 21 = <strong>24 ✅</strong> (Tune yahi solve kiya tha!)</div></div>
    </div>
    <div class="btn-row">
      <button class="btn primary" onclick="showSteps2b()">Steps dikhao</button>
    </div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--pink)"></div>Double Negative Trick 💡</div>
    <p style="font-size:14px;color:var(--muted);line-height:1.7">Jab bhi <strong style="color:var(--red)">minus ke baad minus</strong> aaye, dono milke <strong style="color:var(--green)">plus</strong> ban jaate hain!</p>
    <div class="formula">− (−36) = +36</div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:12px">
      <div style="padding:10px;background:var(--bg3);border-radius:8px;text-align:center;border:1px solid rgba(74,222,128,0.3)"><div style="font-size:12px;color:var(--muted)">Same signs</div><div style="font-size:16px;font-weight:800;color:var(--green);font-family:var(--mono)">+ × + = +<br>− × − = +</div></div>
      <div style="padding:10px;background:var(--bg3);border-radius:8px;text-align:center;border:1px solid rgba(248,113,113,0.3)"><div style="font-size:12px;color:var(--muted)">Different signs</div><div style="font-size:16px;font-weight:800;color:var(--red);font-family:var(--mono)">+ × − = −<br>− × + = −</div></div>
    </div>
  </div>

  <div class="btn-row">
    <button class="btn primary" onclick="goPage('games')">🎮 Practice karo Game mein!</button>
    <button class="btn" onclick="goPage('det3')">Aage jao → 3×3</button>
  </div>
</div>

<!-- 3x3 PAGE -->
<div id="page-det3" class="page">
  <div class="sec-head">
    <h2>3×3 <span>Determinant</span></h2>
    <p>Thoda bada hai but same logic — pehli row se expand karo!</p>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot"></div>Formula</div>
    <div class="formula" style="font-size:12px;line-height:1.8">D = a₁₁·M₁₁ − a₁₂·M₁₂ + a₁₃·M₁₃</div>
    <p style="font-size:13px;color:var(--muted);line-height:1.7;margin-top:8px">
      Pehli row ke teen elements lete hain — har element ko uska 2×2 minor se multiply karte hain — phir <span style="color:var(--green)">+</span> <span style="color:var(--red)">−</span> <span style="color:var(--green)">+</span> karte hain
    </p>
    <div style="display:flex;gap:12px;align-items:center;margin-top:12px;flex-wrap:wrap">
      <div class="matrix-wrap">
        <span class="matrix-bracket">|</span>
        <div class="matrix-grid" style="grid-template-columns:1fr 1fr 1fr">
          <div class="mcell hi-p">a₁₁</div><div class="mcell hi-b">a₁₂</div><div class="mcell hi-g">a₁₃</div>
          <div class="mcell">a₂₁</div><div class="mcell">a₂₂</div><div class="mcell">a₂₃</div>
          <div class="mcell">a₃₁</div><div class="mcell">a₃₂</div><div class="mcell">a₃₃</div>
        </div>
        <span class="matrix-bracket">|</span>
      </div>
      <div style="font-size:13px;color:var(--muted);line-height:2">
        🟣 a₁₁ → <span style="color:var(--green)">+</span> multiply<br>
        🩷 a₁₂ → <span style="color:var(--red)">−</span> multiply<br>
        🟢 a₁₃ → <span style="color:var(--green)">+</span> multiply
      </div>
    </div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--teal)"></div>Example — Step by Step <span class="badge med">Medium</span></div>
    <div style="margin-bottom:12px;font-size:14px;color:var(--muted)">Evaluate karo:</div>
    <div class="matrix-wrap">
      <span class="matrix-bracket">|</span>
      <div class="matrix-grid" style="grid-template-columns:1fr 1fr 1fr">
        <div class="mcell hi-p">3</div><div class="mcell hi-b">−4</div><div class="mcell hi-g">5</div>
        <div class="mcell">1</div><div class="mcell">1</div><div class="mcell">−2</div>
        <div class="mcell">2</div><div class="mcell">3</div><div class="mcell">−1</div>
      </div>
      <span class="matrix-bracket">|</span>
    </div>
    <div class="steps" id="ex3-steps">
      <div class="step" id="s3-1"><div class="step-num">1</div><div class="step-txt"><span style="color:var(--purple)">3</span> × |1,−2; 3,−1| = 3 × ((1×−1)−(−2×3)) = 3 × (−1+6) = 3×5 = <strong>15</strong></div></div>
      <div class="step" id="s3-2"><div class="step-num">2</div><div class="step-txt"><span style="color:var(--pink)">−(−4)</span> × |1,−2; 2,−1| = 4 × ((1×−1)−(−2×2)) = 4 × (−1+4) = 4×3 = <strong>12</strong></div></div>
      <div class="step" id="s3-3"><div class="step-num">3</div><div class="step-txt"><span style="color:var(--green)">5</span> × |1,1; 2,3| = 5 × (3−2) = 5×1 = <strong>5</strong></div></div>
      <div class="step" id="s3-4"><div class="step-num">4</div><div class="step-txt">D = 15 + 12 + 5 = <strong>32 ✅</strong></div></div>
    </div>
    <div class="btn-row">
      <button class="btn primary" id="btn3" onclick="showSteps3()">Steps dikhao</button>
    </div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--amber)"></div>Minor nikalne ka trick 🔑</div>
    <p style="font-size:14px;color:var(--muted);margin-bottom:12px;line-height:1.6">
      Jis element ka minor chahiye, uski <strong style="color:var(--red)">row aur column</strong> mentally cross karo — bacha hua 2×2 determinant = Minor!
    </p>
    <div style="display:flex;gap:16px;align-items:center;flex-wrap:wrap">
      <div>
        <div style="font-size:12px;color:var(--muted);margin-bottom:6px">a₁₁ ka minor → Row 1 & Col 1 hatao:</div>
        <div class="matrix-wrap">
          <span class="matrix-bracket">|</span>
          <div class="matrix-grid" style="grid-template-columns:1fr 1fr 1fr">
            <div class="mcell strike">a₁₁</div><div class="mcell strike">a₁₂</div><div class="mcell strike">a₁₃</div>
            <div class="mcell strike">a₂₁</div><div class="mcell hi-p">a₂₂</div><div class="mcell hi-p">a₂₃</div>
            <div class="mcell strike">a₃₁</div><div class="mcell hi-p">a₃₂</div><div class="mcell hi-p">a₃₃</div>
          </div>
          <span class="matrix-bracket">|</span>
        </div>
      </div>
      <div style="font-size:13px;color:var(--muted)">→ M₁₁ = <span style="font-family:var(--mono);color:var(--purple)">|a₂₂ a₂₃; a₃₂ a₃₃|</span></div>
    </div>
  </div>

  <div class="btn-row">
    <button class="btn primary" onclick="goPage('games')">🎮 Game khelne jao!</button>
    <button class="btn" onclick="goPage('props')">Properties →</button>
  </div>
</div>

<!-- PROPERTIES PAGE -->
<div id="page-props" class="page">
  <div class="sec-head">
    <h2>Properties of <span>Determinants</span></h2>
    <p>5 rules — exam mein bahut kaam aate hain!</p>
  </div>

  <div class="card">
    <div class="prop-item">
      <div class="prop-title">Property 1 — Transpose karo, same rahega</div>
      <div class="prop-desc">Agar rows ko columns bana do (aur columns ko rows) toh determinant ka value nahi badlega</div>
      <div class="prop-example">|A| = |Aᵀ|</div>
    </div>
    <div class="prop-item">
      <div class="prop-title">Property 2 — 2 rows/columns swap karo → sign badlega</div>
      <div class="prop-desc">Koi bhi do rows ya do columns interchange karne pe determinant ka sign badal jaata hai</div>
      <div class="prop-example">Agar R1 ↔ R2 karo → D banega −D</div>
    </div>
    <div class="prop-item">
      <div class="prop-title">Property 3 — Identical rows/columns → Zero!</div>
      <div class="prop-desc">Agar koi bhi do rows ya do columns ek jaisi hain, toh determinant = 0 hoga</div>
      <div class="prop-example">R1 = R2 hoga toh |A| = 0</div>
    </div>
    <div class="prop-item">
      <div class="prop-title">Property 4 — Row ko k se multiply karo</div>
      <div class="prop-desc">Ek poori row ya column ke saare elements k se multiply karo toh determinant bhi k guna ho jaata hai</div>
      <div class="prop-example">Ek row ko 3 se multiply karo → D banega 3D</div>
    </div>
    <div class="prop-item">
      <div class="prop-title">Property 5 — Sum wale elements → 2 determinants</div>
      <div class="prop-desc">Agar kisi row ka element 2 numbers ka sum hai, toh determinant 2 alag determinants mein split ho sakta hai</div>
      <div class="prop-example">|a+b, c; d, e| = |a,c; d,e| + |b,c; d,e|</div>
    </div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--amber)"></div>Quick Quiz — Kaunsa property?</div>
    <div id="prop-quiz">
      <div class="q-box">
        <div class="q-text" id="pq-text">Ek determinant mein R1 aur R2 same hain. Value kya hogi?</div>
        <div class="options">
          <div class="opt" onclick="checkProp(this, false)">1</div>
          <div class="opt" onclick="checkProp(this, true)">0</div>
          <div class="opt" onclick="checkProp(this, false)">−1</div>
          <div class="opt" onclick="checkProp(this, false)">Kuch bhi</div>
        </div>
        <div class="feedback" id="pq-fb"></div>
      </div>
    </div>
  </div>

  <div class="btn-row">
    <button class="btn primary" onclick="goPage('minor')">Minor & Cofactor →</button>
  </div>
</div>

<!-- MINOR & COFACTOR -->
<div id="page-minor" class="page">
  <div class="sec-head">
    <h2>Minor & <span>Cofactor</span></h2>
    <p>Thoda confusing lagta hai pehle, but ek baar samjh gaye toh easy hai!</p>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot"></div>Minor kya hota hai?</div>
    <p style="font-size:14px;color:var(--muted);line-height:1.7;margin-bottom:10px">
      Kisi element aᵢⱼ ka <strong style="color:var(--purple)">Minor (Mᵢⱼ)</strong> = us element ki <strong style="color:var(--red)">i-th row aur j-th column hatao</strong>, bacha hua determinant nikalo
    </p>
    <div class="formula">Minor of aᵢⱼ = Mᵢⱼ</div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--pink)"></div>Cofactor kya hota hai?</div>
    <p style="font-size:14px;color:var(--muted);line-height:1.7;margin-bottom:10px">
      Cofactor = Minor ke saath <strong style="color:var(--amber)">sign lagao</strong>. Sign depend karta hai position pe — (−1)^(i+j)
    </p>
    <div class="formula">Cᵢⱼ = (−1)^(i+j) × Mᵢⱼ</div>

    <div style="margin-top:14px">
      <div style="font-size:13px;font-weight:700;margin-bottom:8px;color:var(--text)">Sign pattern (yaad rakhna ✅):</div>
      <div style="display:flex;align-items:center;gap:16px;flex-wrap:wrap">
        <div class="sign-grid">
          <div class="sign-cell sign-plus">+</div>
          <div class="sign-cell sign-minus">−</div>
          <div class="sign-cell sign-plus">+</div>
          <div class="sign-cell sign-minus">−</div>
          <div class="sign-cell sign-plus">+</div>
          <div class="sign-cell sign-minus">−</div>
          <div class="sign-cell sign-plus">+</div>
          <div class="sign-cell sign-minus">−</div>
          <div class="sign-cell sign-plus">+</div>
        </div>
        <div style="font-size:12px;color:var(--muted);line-height:1.8">
          Position (1,1) → (+)<br>
          Position (1,2) → (−)<br>
          Position (2,1) → (−)<br>
          Corners = Always (+)<br>
          <strong style="color:var(--amber)">Chess board jaisa pattern!</strong>
        </div>
      </div>
    </div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--teal)"></div>Example — Find M₂₃ of A <span class="badge med">Medium</span></div>
    <div style="margin-bottom:12px">
      <div class="matrix-wrap">
        <span class="matrix-bracket">|</span>
        <div class="matrix-grid" style="grid-template-columns:1fr 1fr 1fr">
          <div class="mcell hi-p">1</div><div class="mcell hi-p">2</div><div class="mcell hi-p">3</div>
          <div class="mcell hi-p">4</div><div class="mcell hi-p">5</div><div class="mcell strike">6</div>
          <div class="mcell hi-p">7</div><div class="mcell hi-p">8</div><div class="mcell hi-p">9</div>
        </div>
        <span class="matrix-bracket">|</span>
      </div>
    </div>
    <div class="steps" id="mc-steps">
      <div class="step" id="mc-1"><div class="step-num">1</div><div class="step-txt">Element 6 hai row 2, column 3 pe (position 2,3)</div></div>
      <div class="step" id="mc-2"><div class="step-num">2</div><div class="step-txt">Row 2 aur Column 3 delete karo → bacha: |1,2; 7,8|</div></div>
      <div class="step" id="mc-3"><div class="step-num">3</div><div class="step-txt">M₂₃ = (1×8)−(2×7) = 8−14 = <strong>−6</strong></div></div>
      <div class="step" id="mc-4"><div class="step-num">4</div><div class="step-txt">C₂₃ = (−1)^(2+3) × (−6) = (−1)⁵ × (−6) = (−1)×(−6) = <strong>+6 ✅</strong></div></div>
    </div>
    <div class="btn-row">
      <button class="btn primary" id="btn-mc" onclick="showMC()">Steps dikhao</button>
    </div>
  </div>

  <div class="btn-row">
    <button class="btn primary" onclick="goPage('cramer')">Cramer's Rule →</button>
  </div>
</div>

<!-- CRAMER'S RULE -->
<div id="page-cramer" class="page">
  <div class="sec-head">
    <h2>Cramer's <span>Rule</span></h2>
    <p>Linear equations solve karne ka systematic method — determinants use karo!</p>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot"></div>2 Variables ke liye</div>
    <p style="font-size:13px;color:var(--muted);margin-bottom:10px;line-height:1.6">Given: a₁x + b₁y = c₁ &nbsp;|&nbsp; a₂x + b₂y = c₂</p>
    <table class="cramer-table">
      <tr><th>Matrix</th><th>Kya hai?</th><th>Kaise banate hain?</th></tr>
      <tr><td>D</td><td>Coefficient matrix</td><td>Coefficients of x, y</td></tr>
      <tr><td>Dₓ</td><td>x-matrix</td><td>x column mein c values daalo</td></tr>
      <tr><td>Dᵧ</td><td>y-matrix</td><td>y column mein c values daalo</td></tr>
    </table>
    <div class="formula">x = Dₓ/D &nbsp;&nbsp;&nbsp; y = Dᵧ/D</div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--teal)"></div>Example <span class="badge easy">Easy</span></div>
    <p style="font-size:14px;color:var(--muted);margin-bottom:12px">Solve: <span style="font-family:var(--mono);color:var(--text)">4x − 3y = 11 &nbsp;|&nbsp; 6x + 5y = 7</span></p>
    <div class="steps" id="cr-steps">
      <div class="step" id="cr-1"><div class="step-num">1</div><div class="step-txt">D = |4,−3; 6,5| = (4×5)−(−3×6) = 20+18 = <strong>38</strong></div></div>
      <div class="step" id="cr-2"><div class="step-num">2</div><div class="step-txt">Dₓ = |11,−3; 7,5| = (11×5)−(−3×7) = 55+21 = <strong>76</strong></div></div>
      <div class="step" id="cr-3"><div class="step-num">3</div><div class="step-txt">Dᵧ = |4,11; 6,7| = (4×7)−(11×6) = 28−66 = <strong>−38</strong></div></div>
      <div class="step" id="cr-4"><div class="step-num">4</div><div class="step-txt">x = Dₓ/D = 76/38 = <strong>2</strong> &nbsp;|&nbsp; y = Dᵧ/D = −38/38 = <strong>−1</strong></div></div>
      <div class="step" id="cr-5"><div class="step-num">✅</div><div class="step-txt">Answer: <strong>(x, y) = (2, −1)</strong></div></div>
    </div>
    <div class="btn-row">
      <button class="btn primary" id="btn-cr" onclick="showCramer()">Steps dikhao</button>
    </div>
  </div>

  <div class="card">
    <div class="card-title"><div class="dot" style="background:var(--amber)"></div>3 Variables ke liye</div>
    <p style="font-size:13px;color:var(--muted);margin-bottom:10px;line-height:1.6">Same concept — ek aur matrix Dᵤ add hota hai!</p>
    <div class="formula" style="font-size:12px">x = Dₓ/D &nbsp;&nbsp; y = Dᵧ/D &nbsp;&nbsp; z = Dᵤ/D</div>
    <div class="tip"><strong>Tip:</strong> Dₓ banate waqt — x ke column ki jagah c values (constants) rakhte hain. Dᵧ mein y column mein c values. Dᵤ mein z column mein c values.</div>
  </div>

  <div class="btn-row">
    <button class="btn primary" onclick="goPage('games')">🎮 Games khelne jao!</button>
  </div>
</div>

<!-- GAMES PAGE -->
<div id="page-games" class="page">
  <div class="sec-head">
    <h2>🎮 <span>Practice Games</span></h2>
    <p>Teen games hain — khelo aur score badhao!</p>
  </div>

  <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:12px;margin-bottom:1.5rem">
    <div class="hero-card" onclick="startGame('calc')">
      <div class="icon">🧮</div>
      <div class="title">Calculator Challenge</div>
      <div class="desc">Determinants calculate karo — 10 questions!</div>
    </div>
    <div class="hero-card" onclick="startGame('mcq')">
      <div class="icon">❓</div>
      <div class="title">MCQ Quiz</div>
      <div class="desc">Properties & concepts pe MCQ</div>
    </div>
    <div class="hero-card" onclick="startGame('match')">
      <div class="icon">🔗</div>
      <div class="title">Match the Pair</div>
      <div class="desc">Matrix aur uska determinant match karo</div>
    </div>
  </div>

  <!-- CALC GAME -->
  <div id="game-calc" class="game-area" style="display:none">
    <div class="game-title">🧮 Calculator Challenge</div>
    <div class="score-bar">
      <div class="score-item"><div class="val" id="cg-score">0</div><div class="lbl">SCORE</div></div>
      <div class="score-item"><div class="val" id="cg-q">1/10</div><div class="lbl">QUESTION</div></div>
      <div class="score-item"><div class="val" id="cg-streak">0🔥</div><div class="lbl">STREAK</div></div>
    </div>
    <div class="progress-wrap"><div class="progress-bar" id="cg-prog" style="width:10%"></div></div>
    <div id="cg-content"></div>
  </div>

  <!-- MCQ GAME -->
  <div id="game-mcq" class="game-area" style="display:none">
    <div class="game-title">❓ MCQ Quiz</div>
    <div class="score-bar">
      <div class="score-item"><div class="val" id="mq-score">0</div><div class="lbl">SCORE</div></div>
      <div class="score-item"><div class="val" id="mq-q">1/8</div><div class="lbl">QUESTION</div></div>
    </div>
    <div class="progress-wrap"><div class="progress-bar" id="mq-prog" style="width:12.5%"></div></div>
    <div id="mq-content"></div>
  </div>

  <!-- MATCH GAME -->
  <div id="game-match" class="game-area" style="display:none">
    <div class="game-title">🔗 Match the Pair</div>
    <div class="score-bar">
      <div class="score-item"><div class="val" id="mt-score">0</div><div class="lbl">MATCHED</div></div>
      <div class="score-item"><div class="val" id="mt-tries">0</div><div class="lbl">TRIES</div></div>
    </div>
    <p style="font-size:13px;color:var(--muted);margin-bottom:12px">Left side se matrix chuno, right side se uska determinant chuno!</p>
    <div id="mt-content"></div>
  </div>
</div>

<script>
// =================== NAVIGATION ===================
function goPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active'));
  document.getElementById('page-'+id).classList.add('active');
  const map={home:0,det2:1,det3:2,props:3,minor:4,cramer:5,games:6};
  if(map[id]!==undefined) document.querySelectorAll('.nav-btn')[map[id]].classList.add('active');
  window.scrollTo(0,0);
}

// =================== STEP REVEALS ===================
function revealSteps(ids, btnId, btnLabel){
  ids.forEach((id,i)=>{
    setTimeout(()=>{
      const el=document.getElementById(id);
      if(el) el.classList.add('show');
    },i*300);
  });
  if(btnId){
    setTimeout(()=>{
      const b=document.getElementById(btnId);
      if(b){b.textContent='✅ Done!';b.disabled=true;b.style.opacity='0.5';}
    },ids.length*300);
  }
}

function showSteps2(){revealSteps(['s2-1','s2-2','s2-3'],'btn2-unused');}
function showSteps2b(){revealSteps(['s2b-1','s2b-2','s2b-3'],'btn2b-unused');}
function showSteps3(){revealSteps(['s3-1','s3-2','s3-3','s3-4'],'btn3');}
function showMC(){revealSteps(['mc-1','mc-2','mc-3','mc-4'],'btn-mc');}
function showCramer(){revealSteps(['cr-1','cr-2','cr-3','cr-4','cr-5'],'btn-cr');}

// =================== PROP QUIZ ===================
function checkProp(el, correct){
  const fb=document.getElementById('pq-fb');
  document.querySelectorAll('#prop-quiz .opt').forEach(o=>o.classList.add('locked'));
  if(correct){
    el.classList.add('correct');
    fb.textContent='✅ Bilkul sahi! Property 3 — Identical rows hain toh |A| = 0';
    fb.className='feedback show ok';
  } else {
    el.classList.add('wrong');
    fb.textContent='❌ Nahi bhai! Identical rows → determinant = 0 hota hai (Property 3)';
    fb.className='feedback show no';
  }
}

// =================== CALC GAME ===================
var cgQ=0, cgScore=0, cgStreak=0;
var calcQs=[
  {mat:[[3,5],[2,4]], ans:2},
  {mat:[[1,2],[3,4]], ans:-2},
  {mat:[[5,3],[2,1]], ans:-1},
  {mat:[[4,0],[2,3]], ans:12},
  {mat:[[6,2],[3,1]], ans:0},
  {mat:[[2,5],[1,3]], ans:1},
  {mat:[[7,4],[5,3]], ans:1},
  {mat:[[1,0],[0,1]], ans:1},
  {mat:[[3,1],[6,2]], ans:0},
  {mat:[[4,3],[2,1]], ans:-2}
];

function startGame(type){
  document.querySelectorAll('.game-area').forEach(g=>g.style.display='none');
  if(type==='calc'){
    cgQ=0; cgScore=0; cgStreak=0;
    document.getElementById('game-calc').style.display='block';
    renderCalcQ();
  } else if(type==='mcq'){
    mqQ=0; mqScore=0;
    document.getElementById('game-mcq').style.display='block';
    renderMCQ();
  } else if(type==='match'){
    document.getElementById('game-match').style.display='block';
    initMatch();
  }
  document.getElementById('game-'+type).scrollIntoView({behavior:'smooth'});
}

function renderCalcQ(){
  if(cgQ>=calcQs.length){
    showCalcFinish(); return;
  }
  const q=calcQs[cgQ];
  document.getElementById('cg-q').textContent=(cgQ+1)+'/'+calcQs.length;
  document.getElementById('cg-score').textContent=cgScore;
  document.getElementById('cg-streak').textContent=cgStreak+'🔥';
  document.getElementById('cg-prog').style.width=((cgQ+1)/calcQs.length*100)+'%';
  const a=q.mat[0][0], b=q.mat[0][1], c=q.mat[1][0], d=q.mat[1][1];
  document.getElementById('cg-content').innerHTML=`
    <div class="q-box">
      <div class="q-text" style="text-align:center">Ye determinant calculate karo:</div>
      <div style="display:flex;justify-content:center;margin:16px 0">
        <div class="matrix-wrap">
          <span class="matrix-bracket">|</span>
          <div class="matrix-grid" style="grid-template-columns:1fr 1fr">
            <div class="mcell">${a}</div><div class="mcell">${b}</div>
            <div class="mcell">${c}</div><div class="mcell">${d}</div>
          </div>
          <span class="matrix-bracket">|</span>
        </div>
      </div>
      <input class="calc-input" id="cg-input" type="number" placeholder="Answer daalo..." onkeydown="if(event.key==='Enter')checkCalc()">
      <div class="btn-row" style="justify-content:center">
        <button class="btn primary" onclick="checkCalc()">Check karo ✓</button>
        <button class="btn" onclick="skipCalc()">Skip →</button>
      </div>
      <div class="feedback" id="cg-fb"></div>
    </div>`;
  document.getElementById('cg-input').focus();
}

function checkCalc(){
  const inp=document.getElementById('cg-input');
  const fb=document.getElementById('cg-fb');
  const val=parseInt(inp.value);
  const correct=calcQs[cgQ].ans;
  if(isNaN(val)){fb.textContent='Kuch toh daalo bhai!';fb.className='feedback show no';return;}
  if(val===correct){
    inp.classList.add('correct');
    cgScore+=10+(cgStreak>=2?5:0); cgStreak++;
    fb.textContent='🎉 Bilkul sahi! +10'+(cgStreak>=2?' +5 streak bonus!':'');
    fb.className='feedback show ok';
  } else {
    inp.classList.add('wrong');
    cgStreak=0;
    const a=calcQs[cgQ].mat[0][0],b=calcQs[cgQ].mat[0][1],c=calcQs[cgQ].mat[1][0],d=calcQs[cgQ].mat[1][1];
    fb.textContent=`❌ Sahi answer: ${correct}. Formula: (${a}×${d}) − (${b}×${c}) = ${a*d} − ${b*c} = ${correct}`;
    fb.className='feedback show no';
  }
  document.querySelector('#cg-content .btn').disabled=true;
  document.querySelector('#cg-content .btn').textContent='✓ Done';
  setTimeout(()=>{cgQ++; renderCalcQ();},1800);
}

function skipCalc(){
  cgStreak=0; cgQ++; renderCalcQ();
}

function showCalcFinish(){
  const pct=Math.round(cgScore/100*100);
  const stars=cgScore>=80?'⭐⭐⭐':cgScore>=50?'⭐⭐':'⭐';
  const msg=cgScore>=80?'Ekdum zabardast! 🔥':cgScore>=50?'Theek thak! Aur practice karo 💪':'Koi baat nahi, dobara try karo! 📚';
  document.getElementById('cg-content').innerHTML=`
    <div class="finish">
      <div class="stars">${stars}</div>
      <div class="big-score">${cgScore}</div>
      <p>${msg}</p>
      <div class="btn-row" style="justify-content:center">
        <button class="btn primary" onclick="startGame('calc')">Dobara khelo 🔄</button>
        <button class="btn" onclick="startGame('mcq')">MCQ Quiz →</button>
      </div>
    </div>`;
}

// =================== MCQ GAME ===================
var mqQ=0, mqScore=0;
var mcqQs=[
  {q:"2×2 determinant |a,b;c,d| ki value kya hai?", opts:["ab+cd","ad−bc","ac−bd","ad+bc"], ans:1, exp:"Formula hai: ad − bc (diagonal multiply karo phir minus)"},
  {q:"Agar koi do rows identical hain toh determinant kya hoga?", opts:["1","-1","0","Kuch bhi"], ans:2, exp:"Property 3: Identical rows/columns → |A| = 0"},
  {q:"Minor M₁₁ matlab kya hai?", opts:["Row 1 aur Col 1 rakhna","Row 1 aur Col 1 hatana","Sirf Row 1 hatana","Sirf Col 1 hatana"], ans:1, exp:"Minor = us element ki row aur column HATAO, baaki ka determinant"},
  {q:"Cofactor C₁₂ mein sign kya aayega?", opts:["(−1)^1 = +","(−1)^2 = +","(−1)^3 = −","(−1)^4 = +"], ans:2, exp:"C₁₂ = (−1)^(1+2) = (−1)^3 = −1, toh minus sign lagega"},
  {q:"Cramer's Rule mein x = ?", opts:["Dy/D","Dx/D","D/Dx","Dx×D"], ans:1, exp:"x = Dx/D — x ka determinant divided by coefficient determinant"},
  {q:"Agar do rows swap karen toh determinant kya hoga?", opts:["Same rahega","Zero ho jayega","Sign badlega","Double ho jayega"], ans:2, exp:"Property 2: Two rows interchange → sign of D changes, D becomes −D"},
  {q:"|4,2;2,1| = ?", opts:["0","4","8","-4"], ans:0, exp:"(4×1)−(2×2) = 4−4 = 0. Identical columns hain almost!"},
  {q:"3×3 determinant expand karte hain:", opts:["Kisi bhi row se","Sirf pehli row se","Sirf teesri column se","Diagonal se"], ans:0, exp:"Kisi bhi row ya column se expand kar sakte hain — mostly pehli row use karte hain"}
];

function renderMCQ(){
  if(mqQ>=mcqQs.length){showMCQFinish(); return;}
  const q=mcqQs[mqQ];
  document.getElementById('mq-q').textContent=(mqQ+1)+'/'+mcqQs.length;
  document.getElementById('mq-score').textContent=mqScore;
  document.getElementById('mq-prog').style.width=((mqQ+1)/mcqQs.length*100)+'%';
  const opts=q.opts.map((o,i)=>`<div class="opt" onclick="checkMCQ(this,${i})">${o}</div>`).join('');
  document.getElementById('mq-content').innerHTML=`
    <div class="q-box">
      <div class="q-text">${q.q}</div>
      <div class="options">${opts}</div>
      <div class="feedback" id="mq-fb"></div>
    </div>`;
}

function checkMCQ(el, idx){
  const q=mcqQs[mqQ];
  document.querySelectorAll('#mq-content .opt').forEach(o=>o.classList.add('locked'));
  const fb=document.getElementById('mq-fb');
  if(idx===q.ans){
    el.classList.add('correct'); mqScore+=10;
    fb.textContent='✅ Sahi hai! '+q.exp;
    fb.className='feedback show ok';
  } else {
    el.classList.add('wrong');
    document.querySelectorAll('#mq-content .opt')[q.ans].classList.add('correct');
    fb.textContent='❌ Galat. '+q.exp;
    fb.className='feedback show no';
  }
  document.getElementById('mq-score').textContent=mqScore;
  setTimeout(()=>{mqQ++; renderMCQ();},2000);
}

function showMCQFinish(){
  const stars=mqScore>=70?'⭐⭐⭐':mqScore>=50?'⭐⭐':'⭐';
  const msg=mqScore>=70?'Concepts bilkul clear hain! 🎓':mqScore>=50?'Concepts seedhe karo thoda 📖':'Notes dobara padho! 📚';
  document.getElementById('mq-content').innerHTML=`
    <div class="finish">
      <div class="stars">${stars}</div>
      <div class="big-score">${mqScore}/80</div>
      <p>${msg}</p>
      <div class="btn-row" style="justify-content:center">
        <button class="btn primary" onclick="startGame('mcq')">Dobara khelo 🔄</button>
        <button class="btn" onclick="startGame('match')">Match Game →</button>
      </div>
    </div>`;
}

// =================== MATCH GAME ===================
var mtPairs=[
  {mat:"|3 5|\n|2 4|", val:"2"},
  {mat:"|1 2|\n|3 4|", val:"-2"},
  {mat:"|4 0|\n|2 3|", val:"12"},
  {mat:"|6 2|\n|3 1|", val:"0"},
  {mat:"|2 5|\n|1 3|", val:"1"},
];
var mtSelected=null, mtMatchedCount=0, mtTries=0;

function initMatch(){
  mtSelected=null; mtMatchedCount=0; mtTries=0;
  document.getElementById('mt-score').textContent=0;
  document.getElementById('mt-tries').textContent=0;
  const shuffledVals=[...mtPairs.map(p=>p.val)].sort(()=>Math.random()-0.5);
  let html='<div class="match-grid">';
  mtPairs.forEach((p,i)=>{
    html+=`<div class="match-item" data-type="mat" data-idx="${i}" onclick="selectMatch(this)">${p.mat.replace('\n','<br>')}</div>`;
  });
  shuffledVals.forEach((v,i)=>{
    html+=`<div class="match-item" data-type="val" data-val="${v}" onclick="selectMatch(this)">${v}</div>`;
  });
  html+='</div><div id="mt-fb" class="feedback" style="margin-top:10px"></div>';
  document.getElementById('mt-content').innerHTML=html;
}

function selectMatch(el){
  if(el.classList.contains('matched')) return;
  const fb=document.getElementById('mt-fb');
  fb.className='feedback';
  if(!mtSelected){
    if(el.classList.contains('selected')){el.classList.remove('selected'); mtSelected=null; return;}
    document.querySelectorAll('.match-item.selected').forEach(e=>e.classList.remove('selected'));
    el.classList.add('selected');
    mtSelected=el;
  } else {
    if(el===mtSelected){el.classList.remove('selected'); mtSelected=null; return;}
    mtTries++;
    document.getElementById('mt-tries').textContent=mtTries;
    const a=mtSelected, b=el;
    const matEl = a.dataset.type==='mat'?a:b;
    const valEl = a.dataset.type==='val'?a:b;
    if(!matEl||!valEl||matEl.dataset.type==='val'||valEl.dataset.type==='mat'){
      a.classList.remove('selected'); mtSelected=null;
      fb.textContent='Pehle matrix chuno, phir uska value!';
      fb.className='feedback show no';
      return;
    }
    const idx=parseInt(matEl.dataset.idx);
    const correctVal=mtPairs[idx].val;
    if(valEl.dataset.val===correctVal){
      matEl.classList.remove('selected'); matEl.classList.add('matched');
      valEl.classList.add('matched');
      mtMatchedCount++;
      document.getElementById('mt-score').textContent=mtMatchedCount;
      fb.textContent='✅ Sahi match!';
      fb.className='feedback show ok';
      if(mtMatchedCount===mtPairs.length){
        setTimeout(()=>{
          document.getElementById('mt-content').innerHTML=`<div class="finish"><div class="stars">⭐⭐⭐</div><div class="big-score">Done!</div><p>Saare pairs match kar liye ${mtTries} tries mein! 🎉</p><div class="btn-row" style="justify-content:center"><button class="btn primary" onclick="initMatch()">Dobara khelo 🔄</button></div></div>`;
        },1000);
      }
    } else {
      a.classList.remove('selected');
      b.classList.add('wrong-sel');
      setTimeout(()=>b.classList.remove('wrong-sel'),600);
      fb.textContent='❌ Galat pair! Dobara try karo.';
      fb.className='feedback show no';
    }
    mtSelected=null;
  }
}
</script>
</body>
</html>
