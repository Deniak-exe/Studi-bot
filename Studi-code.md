<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Studi-Bot | Aprende, Juega y Diviértete</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&family=Exo+2:wght@300;400;600;700&display=swap');

:root {
  --bg-deep: #020b18;
  --bg-dark: #050f1e;
  --bg-card: #071428;
  --bg-card2: #0a1a32;
  --cyan: #00e5ff;
  --cyan-dim: #00b8cc;
  --cyan-glow: rgba(0,229,255,0.15);
  --blue: #1565c0;
  --blue-light: #1e88e5;
  --blue-bright: #42a5f5;
  --accent: #00e5ff;
  --accent2: #0097a7;
  --gold: #ffd600;
  --ram: #a0f0a0;
  --red: #f44336;
  --green: #00e676;
  --white: #e0f7fa;
  --text-dim: #7ecdd8;
  --border: rgba(0,229,255,0.2);
  --border-bright: rgba(0,229,255,0.5);
  --shadow-cyan: 0 0 20px rgba(0,229,255,0.3);
  --shadow-deep: 0 8px 32px rgba(0,0,0,0.6);
  --font-title: 'Orbitron', monospace;
  --font-mono: 'Share Tech Mono', monospace;
  --font-body: 'Exo 2', sans-serif;
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  background: var(--bg-deep);
  color: var(--white);
  font-family: var(--font-body);
  min-height: 100vh;
  overflow-x: hidden;
  position: relative;
}

/* ====== FLOATING BG ELEMENTS ====== */
#bg-canvas {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  pointer-events: none;
  z-index: 0;
  opacity: 0.18;
}

/* ====== SCREENS ====== */
.screen {
  position: relative;
  z-index: 1;
  min-height: 100vh;
  display: none;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}
.screen.active { display: flex; }

/* ====== SCROLLBAR ====== */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--bg-dark); }
::-webkit-scrollbar-thumb { background: var(--cyan-dim); border-radius: 3px; }

/* ====== HEADER BADGE ====== */
.logo-badge {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 8px;
}
.logo-icon {
  width: 56px; height: 56px;
  background: linear-gradient(135deg, var(--cyan), var(--blue-light));
  border-radius: 14px;
  display: flex; align-items: center; justify-content: center;
  font-size: 28px;
  box-shadow: var(--shadow-cyan);
  animation: pulse-glow 2.5s ease-in-out infinite;
}
@keyframes pulse-glow {
  0%,100% { box-shadow: 0 0 12px rgba(0,229,255,0.4); }
  50% { box-shadow: 0 0 28px rgba(0,229,255,0.8); }
}

/* ====== TITLE ====== */
.main-title {
  font-family: var(--font-title);
  font-size: clamp(2.2rem, 6vw, 4rem);
  font-weight: 900;
  background: linear-gradient(90deg, var(--cyan), #fff, var(--cyan));
  background-size: 200%;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: shimmer 3s linear infinite;
  letter-spacing: 2px;
  text-align: center;
}
@keyframes shimmer {
  0% { background-position: 0% 50%; }
  100% { background-position: 200% 50%; }
}
.sub-title {
  font-family: var(--font-mono);
  color: var(--cyan-dim);
  font-size: 1.1rem;
  text-align: center;
  letter-spacing: 4px;
  margin-top: 4px;
  margin-bottom: 16px;
}
.desc-text {
  color: var(--text-dim);
  font-size: 0.95rem;
  text-align: center;
  max-width: 560px;
  line-height: 1.7;
  margin-bottom: 32px;
}

/* ====== STATS BAR (top) ====== */
.stats-bar {
  display: flex;
  gap: 16px;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 20px;
}
.stat-chip {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 30px;
  padding: 6px 18px;
  display: flex;
  align-items: center;
  gap: 8px;
  font-family: var(--font-mono);
  font-size: 1rem;
  color: var(--white);
  transition: all 0.3s;
}
.stat-chip .icon { font-size: 1.2rem; }
.stat-chip.score .val { color: var(--cyan); font-weight: 700; }
.stat-chip.lives .val { color: #ff6b6b; }
.stat-chip.ram .val { color: var(--ram); }
.stat-chip.record .val { color: var(--gold); }

/* ====== WORLD CARDS ====== */
.worlds-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 20px;
  width: 100%;
  max-width: 960px;
  margin-bottom: 32px;
}
.world-card {
  background: linear-gradient(145deg, var(--bg-card), var(--bg-card2));
  border: 1px solid var(--border);
  border-radius: 18px;
  padding: 28px 20px;
  cursor: pointer;
  transition: all 0.35s cubic-bezier(.4,0,.2,1);
  position: relative;
  overflow: hidden;
  text-align: center;
}
.world-card::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, var(--cyan-glow), transparent);
  opacity: 0;
  transition: opacity 0.3s;
}
.world-card:hover::before { opacity: 1; }
.world-card:hover {
  border-color: var(--border-bright);
  transform: translateY(-5px);
  box-shadow: var(--shadow-cyan), var(--shadow-deep);
}
.world-card:hover .world-icon { transform: scale(1.15); }
.world-icon {
  font-size: 3rem;
  margin-bottom: 14px;
  display: block;
  transition: transform 0.3s;
  filter: drop-shadow(0 0 8px var(--cyan));
}
.world-name {
  font-family: var(--font-title);
  font-size: 0.95rem;
  color: var(--cyan);
  letter-spacing: 1px;
  margin-bottom: 8px;
}
.world-desc {
  font-size: 0.8rem;
  color: var(--text-dim);
  line-height: 1.5;
}
.world-levels {
  margin-top: 12px;
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--blue-bright);
}
.world-lock {
  position: absolute;
  top: 10px; right: 14px;
  font-size: 1.1rem;
  color: var(--gold);
}

/* ====== BUTTONS ====== */
.btn {
  font-family: var(--font-title);
  font-size: 0.85rem;
  letter-spacing: 1.5px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.25s;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
  text-transform: uppercase;
}
.btn-primary {
  background: linear-gradient(135deg, var(--cyan), var(--blue-light));
  color: var(--bg-deep);
  font-weight: 700;
  box-shadow: 0 4px 16px rgba(0,229,255,0.35);
}
.btn-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 24px rgba(0,229,255,0.5); }
.btn-secondary {
  background: transparent;
  border: 1px solid var(--border-bright);
  color: var(--cyan);
}
.btn-secondary:hover { background: var(--cyan-glow); }
.btn-danger {
  background: linear-gradient(135deg, #c62828, #e53935);
  color: #fff;
}
.btn-icon {
  background: var(--bg-card);
  border: 1px solid var(--border);
  color: var(--cyan);
  padding: 10px 14px;
  border-radius: 10px;
  font-size: 1.1rem;
}
.btn-icon:hover { border-color: var(--cyan); background: var(--cyan-glow); }

/* ====== LESSON SCREEN ====== */
.lesson-header {
  text-align: center;
  margin-bottom: 28px;
}
.lesson-world-icon { font-size: 4rem; margin-bottom: 10px; display: block; filter: drop-shadow(0 0 10px var(--cyan)); }
.lesson-title { font-family: var(--font-title); font-size: 1.6rem; color: var(--cyan); margin-bottom: 6px; }
.lesson-subtitle { color: var(--text-dim); font-size: 0.95rem; }

.key-idea {
  background: linear-gradient(135deg, rgba(0,150,200,0.15), rgba(0,50,100,0.2));
  border: 1px solid var(--border-bright);
  border-left: 4px solid var(--cyan);
  border-radius: 12px;
  padding: 18px 22px;
  margin-bottom: 28px;
  max-width: 700px;
  width: 100%;
}
.key-idea-label {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--cyan);
  letter-spacing: 3px;
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 6px;
}
.key-idea p { font-size: 0.92rem; line-height: 1.7; color: var(--white); }

.concepts-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 14px;
  width: 100%;
  max-width: 700px;
  margin-bottom: 30px;
}
.concept-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 16px;
  text-align: center;
  transition: all 0.3s;
}
.concept-card:hover { border-color: var(--cyan); transform: translateY(-3px); }
.concept-icon { font-size: 1.8rem; margin-bottom: 8px; display: block; }
.concept-term { font-family: var(--font-mono); font-size: 0.78rem; color: var(--cyan); margin-bottom: 4px; }
.concept-def { font-size: 0.75rem; color: var(--text-dim); line-height: 1.4; }

/* ====== GAME SCREEN ====== */
.game-topbar {
  width: 100%;
  max-width: 820px;
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 14px;
  flex-wrap: wrap;
}
.game-topbar .btn-icon { font-size: 1rem; padding: 8px 12px; }

.progress-outer {
  flex: 1;
  height: 8px;
  background: rgba(255,255,255,0.08);
  border-radius: 4px;
  overflow: hidden;
  min-width: 80px;
}
.progress-inner {
  height: 100%;
  background: linear-gradient(90deg, var(--cyan), var(--blue-bright));
  border-radius: 4px;
  transition: width 0.6s ease;
  box-shadow: 0 0 8px var(--cyan);
}

.timer-wrap {
  position: relative;
  width: 48px; height: 48px;
}
.timer-svg { transform: rotate(-90deg); }
.timer-bg { fill: none; stroke: rgba(255,255,255,0.08); stroke-width: 4; }
.timer-bar { fill: none; stroke: var(--cyan); stroke-width: 4; stroke-linecap: round; transition: stroke-dashoffset 1s linear, stroke 0.5s; }
.timer-text {
  position: absolute; inset: 0;
  display: flex; align-items: center; justify-content: center;
  font-family: var(--font-mono);
  font-size: 0.85rem;
  color: var(--white);
}

.level-card {
  background: linear-gradient(145deg, var(--bg-card), var(--bg-card2));
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 28px;
  width: 100%;
  max-width: 820px;
  position: relative;
  overflow: hidden;
}
.level-card::after {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--cyan), transparent);
}

.level-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(0,229,255,0.08);
  border: 1px solid var(--border);
  border-radius: 30px;
  padding: 4px 14px;
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--cyan);
  margin-bottom: 16px;
}

.mission-label {
  font-family: var(--font-mono);
  font-size: 0.7rem;
  color: var(--blue-bright);
  letter-spacing: 3px;
  margin-bottom: 6px;
}
.mission-text {
  font-family: var(--font-title);
  font-size: 1.1rem;
  color: var(--white);
  margin-bottom: 18px;
  line-height: 1.4;
}

.fragment-box {
  background: rgba(0,0,0,0.25);
  border: 1px solid rgba(0,229,255,0.1);
  border-radius: 12px;
  padding: 16px;
  font-size: 0.88rem;
  color: #b0d8e8;
  line-height: 1.7;
  margin-bottom: 18px;
  font-family: var(--font-body);
  position: relative;
}
.fragment-box .frag-icon {
  position: absolute;
  top: -10px; left: 14px;
  background: var(--bg-card);
  padding: 0 6px;
  font-size: 1rem;
}

.hint-row {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 16px;
  flex-wrap: wrap;
}
.hint-box {
  flex: 1;
  background: rgba(255, 214, 0, 0.06);
  border: 1px dashed rgba(255,214,0,0.3);
  border-radius: 10px;
  padding: 10px 14px;
  font-size: 0.82rem;
  color: #ffe082;
  font-family: var(--font-mono);
  min-height: 38px;
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: default;
}
.hint-reveal-btn {
  background: rgba(255,214,0,0.1);
  border: 1px solid rgba(255,214,0,0.4);
  color: var(--gold);
  font-family: var(--font-mono);
  font-size: 0.75rem;
  padding: 8px 14px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
  white-space: nowrap;
}
.hint-reveal-btn:hover { background: rgba(255,214,0,0.2); }

.question-text {
  font-family: var(--font-body);
  font-size: 1.05rem;
  font-weight: 600;
  color: var(--white);
  margin-bottom: 20px;
  line-height: 1.5;
  padding: 14px 18px;
  background: rgba(0,229,255,0.05);
  border-radius: 10px;
  border-left: 3px solid var(--cyan);
}

.options-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
  margin-bottom: 16px;
}
@media (max-width: 520px) { .options-grid { grid-template-columns: 1fr; } }

.option-btn {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 14px 18px;
  text-align: left;
  color: var(--white);
  font-family: var(--font-body);
  font-size: 0.88rem;
  cursor: pointer;
  transition: all 0.2s;
  display: flex;
  gap: 10px;
  align-items: flex-start;
  line-height: 1.4;
}
.option-btn:hover:not(:disabled) {
  border-color: var(--cyan);
  background: var(--cyan-glow);
  transform: translateX(4px);
}
.option-btn .opt-letter {
  font-family: var(--font-mono);
  color: var(--cyan);
  font-weight: 700;
  min-width: 20px;
}
.option-btn.correct { border-color: var(--green); background: rgba(0,230,118,0.1); }
.option-btn.wrong { border-color: var(--red); background: rgba(244,67,54,0.1); }
.option-btn:disabled { cursor: default; }

/* ====== FEEDBACK MODAL ====== */
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(2,11,24,0.88);
  z-index: 100;
  display: none;
  align-items: center;
  justify-content: center;
  padding: 20px;
  backdrop-filter: blur(4px);
}
.modal-overlay.active { display: flex; }
.modal-box {
  background: linear-gradient(145deg, var(--bg-card), var(--bg-card2));
  border-radius: 22px;
  padding: 36px 32px;
  max-width: 520px;
  width: 100%;
  position: relative;
  text-align: center;
  border: 1px solid var(--border);
  animation: modal-in 0.35s cubic-bezier(.4,0,.2,1);
}
@keyframes modal-in {
  from { transform: scale(0.85) translateY(20px); opacity: 0; }
  to { transform: scale(1) translateY(0); opacity: 1; }
}
.modal-result-icon { font-size: 4rem; margin-bottom: 12px; display: block; }
.modal-result-title { font-family: var(--font-title); font-size: 1.4rem; margin-bottom: 10px; }
.modal-result-title.ok { color: var(--green); }
.modal-result-title.fail { color: var(--red); }
.modal-explanation {
  color: #b0d8e8;
  font-size: 0.88rem;
  line-height: 1.7;
  margin-bottom: 14px;
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 14px;
  text-align: left;
}
.modal-remember {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--gold);
  margin-bottom: 20px;
  padding: 10px;
  border: 1px dashed rgba(255,214,0,0.3);
  border-radius: 8px;
}
.modal-score-gained {
  font-family: var(--font-title);
  font-size: 1.6rem;
  color: var(--cyan);
  margin-bottom: 18px;
}

/* ====== PAUSE MODAL ====== */
.pause-box {
  max-width: 360px;
  text-align: center;
}
.pause-title {
  font-family: var(--font-title);
  font-size: 1.6rem;
  color: var(--cyan);
  margin-bottom: 20px;
}
.vol-label {
  font-family: var(--font-mono);
  font-size: 0.78rem;
  color: var(--text-dim);
  margin-bottom: 8px;
  letter-spacing: 2px;
}
input[type=range] {
  width: 100%;
  accent-color: var(--cyan);
  margin-bottom: 20px;
}
.pause-btns { display: flex; flex-direction: column; gap: 12px; }

/* ====== GAME OVER / WIN MODALS ====== */
.gameover-box {
  max-width: 460px;
  text-align: center;
}
.big-icon { font-size: 5rem; margin-bottom: 12px; display: block; }
.go-title { font-family: var(--font-title); font-size: 2rem; margin-bottom: 8px; }
.go-stats { display: flex; flex-direction: column; gap: 8px; margin: 20px 0; }
.go-stat {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(0,0,0,0.2);
  border-radius: 8px;
  padding: 10px 16px;
  font-family: var(--font-mono);
  font-size: 0.88rem;
}
.go-stat .gsl { color: var(--text-dim); }
.go-stat .gsv { color: var(--cyan); font-weight: 700; }

/* ====== CONFETTI ====== */
#confetti-canvas {
  position: fixed; inset: 0;
  pointer-events: none;
  z-index: 200;
}

/* ====== RECORD SCREEN ====== */
.record-card {
  background: linear-gradient(135deg, rgba(255,214,0,0.08), rgba(0,50,80,0.3));
  border: 1px solid rgba(255,214,0,0.3);
  border-radius: 18px;
  padding: 28px;
  text-align: center;
  max-width: 420px;
  width: 100%;
  margin-bottom: 24px;
}
.record-icon { font-size: 3.5rem; margin-bottom: 10px; display: block; }
.record-label {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--gold);
  letter-spacing: 3px;
  margin-bottom: 6px;
}
.record-score {
  font-family: var(--font-title);
  font-size: 3rem;
  color: var(--gold);
  text-shadow: 0 0 20px rgba(255,214,0,0.4);
}

/* ====== SOUND BTN ====== */
#sound-toggle {
  position: fixed;
  top: 16px; right: 16px;
  z-index: 50;
  background: var(--bg-card);
  border: 1px solid var(--border);
  color: var(--cyan);
  width: 42px; height: 42px;
  border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  font-size: 1.2rem;
  cursor: pointer;
  transition: all 0.2s;
}
#sound-toggle:hover { border-color: var(--cyan); background: var(--cyan-glow); }

/* ====== PIXEL GRID OVERLAY ====== */
.pixel-dots {
  position: fixed; inset: 0;
  pointer-events: none;
  z-index: 0;
  background-image: radial-gradient(circle, rgba(0,229,255,0.04) 1px, transparent 1px);
  background-size: 28px 28px;
}

/* ====== WORLD PROGRESS DOTS ====== */
.level-dots {
  display: flex;
  flex-wrap: wrap;
  gap: 4px;
  margin-top: 8px;
  justify-content: center;
}
.level-dot {
  width: 8px; height: 8px;
  border-radius: 2px;
  background: rgba(255,255,255,0.1);
  transition: all 0.3s;
}
.level-dot.done { background: var(--cyan); }
.level-dot.current { background: var(--gold); animation: blink 1s infinite; }
@keyframes blink { 0%,100%{opacity:1;} 50%{opacity:0.4;} }

/* ====== GOAL BAR ====== */
.goal-bar {
  background: rgba(0,229,255,0.06);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 10px 14px;
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--text-dim);
  margin-bottom: 14px;
}
.goal-bar span { color: var(--cyan); }

/* toast */
.toast {
  position: fixed;
  bottom: 20px; left: 50%;
  transform: translateX(-50%) translateY(80px);
  background: var(--bg-card);
  border: 1px solid var(--border-bright);
  border-radius: 30px;
  padding: 10px 24px;
  font-family: var(--font-mono);
  font-size: 0.85rem;
  color: var(--cyan);
  z-index: 500;
  transition: transform 0.4s cubic-bezier(.4,0,.2,1);
  white-space: nowrap;
}
.toast.show { transform: translateX(-50%) translateY(0); }

/* ====== RESPONSIVE ====== */
@media (max-width: 600px) {
  .game-topbar { gap: 6px; }
  .level-card { padding: 18px; }
  .options-grid { gap: 8px; }
  .option-btn { padding: 12px 14px; font-size: 0.82rem; }
}
</style>
</head>
<body>

<div class="pixel-dots"></div>
<canvas id="bg-canvas"></canvas>
<canvas id="confetti-canvas"></canvas>

<button id="sound-toggle" title="Sonido">🔊</button>

<!-- ========== SCREEN: HOME ========== -->
<div class="screen active" id="screen-home">
  <div style="margin-top:40px; margin-bottom:8px; display:flex; flex-direction:column; align-items:center;">
    <div class="logo-badge">
      <div class="logo-icon">🤖</div>
      <div>
        <div class="main-title">Studi-Bot</div>
        <div class="sub-title">&gt; APRENDE · JUEGA · DIVIÉRTETE _</div>
      </div>
    </div>
    <p class="desc-text">Una aventura educativa gamificada sobre <strong style="color:var(--cyan)">Hardware y Redes de Computadores</strong>. Explora mundos, supera niveles, gana RAM y conviértete en experto digital.</p>
  </div>

  <div class="stats-bar">
    <div class="stat-chip score"><span class="icon">⭐</span> Puntaje: <span class="val" id="home-score">0</span></div>
    <div class="stat-chip lives"><span class="icon">❤️</span> Vidas: <span class="val" id="home-lives">3</span></div>
    <div class="stat-chip ram"><span class="icon">💾</span> RAM: <span class="val" id="home-ram">30</span></div>
    <div class="stat-chip record"><span class="icon">🏆</span> Récord: <span class="val" id="home-record">0</span></div>
  </div>

  <div class="worlds-grid" id="worlds-grid"></div>

  <div style="display:flex; gap:12px; flex-wrap:wrap; justify-content:center; margin-bottom:30px;">
    <button class="btn btn-secondary" onclick="showRecord()">🏆 Tabla de Récord</button>
    <button class="btn btn-secondary" onclick="resetAll()">🔄 Reiniciar Todo</button>
  </div>
</div>

<!-- ========== SCREEN: LESSON ========== -->
<div class="screen" id="screen-lesson">
  <div class="lesson-header">
    <span class="lesson-world-icon" id="lesson-icon">📡</span>
    <div class="lesson-title" id="lesson-title">Mundo 1</div>
    <div class="lesson-subtitle" id="lesson-subtitle">Descripción</div>
  </div>
  <div class="key-idea" id="lesson-key-idea">
    <div class="key-idea-label">⚡ IDEA CLAVE</div>
    <p id="lesson-key-text"></p>
  </div>
  <div class="concepts-grid" id="lesson-concepts"></div>
  <div style="display:flex; gap:12px; flex-wrap:wrap; justify-content:center;">
    <button class="btn btn-primary" id="btn-start-game" onclick="startGame()">▶ INICIAR JUEGO</button>
    <button class="btn btn-secondary" onclick="showScreen('screen-home')">← VOLVER</button>
  </div>
</div>

<!-- ========== SCREEN: GAME ========== -->
<div class="screen" id="screen-game">
  <div class="game-topbar">
    <button class="btn-icon" onclick="showScreen('screen-home')" title="Inicio">🏠</button>
    <button class="btn-icon" onclick="openPause()" title="Pausa (P)">⏸</button>
    <div class="progress-outer"><div class="progress-inner" id="prog-bar" style="width:0%"></div></div>
    <div class="stat-chip score" style="padding:4px 12px;"><span class="icon">⭐</span><span class="val" id="game-score">0</span></div>
    <div class="stat-chip lives" style="padding:4px 12px;"><span class="icon">❤️</span><span class="val" id="game-lives">3</span></div>
    <div class="stat-chip ram" style="padding:4px 10px;"><span class="icon">💾</span><span class="val" id="game-ram">30</span></div>
    <div class="timer-wrap">
      <svg class="timer-svg" width="48" height="48" viewBox="0 0 48 48">
        <circle class="timer-bg" cx="24" cy="24" r="20"/>
        <circle class="timer-bar" id="timer-bar" cx="24" cy="24" r="20" stroke-dasharray="125.66" stroke-dashoffset="0"/>
      </svg>
      <div class="timer-text" id="timer-text">30</div>
    </div>
  </div>

  <div class="level-card" id="level-card">
    <div class="level-badge" id="level-badge">🌍 Nivel 1/30</div>
    <div class="goal-bar" id="goal-bar"><span>META:</span> —</div>
    <div class="mission-label">📋 MISIÓN</div>
    <div class="mission-text" id="mission-text">—</div>
    <div class="fragment-box"><span class="frag-icon">📄</span><span id="fragment-text">—</span></div>
    <div class="hint-row">
      <div class="hint-box" id="hint-box">💡 Pulsa el botón para revelar la pista</div>
      <button class="hint-reveal-btn" id="hint-btn" onclick="revealHint()">🔍 PISTA (−25💾)</button>
    </div>
    <div class="question-text" id="question-text">¿Pregunta?</div>
    <div class="options-grid" id="options-container"></div>
  </div>

  <div class="level-dots" id="level-dots"></div>
</div>

<!-- ========== MODAL: FEEDBACK ========== -->
<div class="modal-overlay" id="modal-feedback">
  <div class="modal-box">
    <span class="modal-result-icon" id="fb-icon">✅</span>
    <div class="modal-result-title" id="fb-title">¡Correcto!</div>
    <div class="modal-score-gained" id="fb-score">+10 pts | +1💾</div>
    <div class="modal-explanation" id="fb-explanation">Explicación aquí.</div>
    <div class="modal-remember" id="fb-remember">💡 Recuerda: dato técnico</div>
    <button class="btn btn-primary" id="btn-next" onclick="nextLevel()">SIGUIENTE NIVEL ▶</button>
  </div>
</div>

<!-- ========== MODAL: PAUSE ========== -->
<div class="modal-overlay" id="modal-pause">
  <div class="modal-box pause-box">
    <div class="pause-title">⏸ PAUSA</div>
    <div class="vol-label">VOLUMEN MÚSICA</div>
    <input type="range" id="vol-slider" min="0" max="100" value="60" oninput="setVolume(this.value)">
    <div class="pause-btns">
      <button class="btn btn-primary" onclick="closePause()">▶ CONTINUAR</button>
      <button class="btn btn-secondary" onclick="resetCurrentLevel()">🔄 REINICIAR NIVEL</button>
      <button class="btn btn-danger" onclick="resetAll()">⚠ RESET COMPLETO</button>
      <button class="btn btn-secondary" onclick="showScreen('screen-home')">🏠 MENÚ PRINCIPAL</button>
    </div>
  </div>
</div>

<!-- ========== MODAL: GAME OVER ========== -->
<div class="modal-overlay" id="modal-gameover">
  <div class="modal-box gameover-box">
    <span class="big-icon">💀</span>
    <div class="go-title" style="color:var(--red)">GAME OVER</div>
    <div class="go-stats" id="go-stats"></div>
    <div style="display:flex; gap:12px; justify-content:center; flex-wrap:wrap;">
      <button class="btn btn-primary" onclick="restartFromGameOver()">🔄 REINTENTAR</button>
      <button class="btn btn-secondary" onclick="showScreen('screen-home')">🏠 INICIO</button>
    </div>
  </div>
</div>

<!-- ========== MODAL: WORLD COMPLETE ========== -->
<div class="modal-overlay" id="modal-win">
  <div class="modal-box gameover-box">
    <span class="big-icon">🎉</span>
    <div class="go-title" style="color:var(--cyan)">¡MUNDO COMPLETADO!</div>
    <div class="go-stats" id="win-stats"></div>
    <div style="display:flex; gap:12px; justify-content:center; flex-wrap:wrap;">
      <button class="btn btn-primary" onclick="showScreen('screen-home')">🌍 ELEGIR MUNDO</button>
    </div>
  </div>
</div>

<!-- ========== MODAL: RECORD ========== -->
<div class="modal-overlay" id="modal-record">
  <div class="modal-box">
    <span class="big-icon">🏆</span>
    <div class="go-title" style="color:var(--gold)">TABLA DE RÉCORD</div>
    <div class="record-card">
      <span class="record-icon">🥇</span>
      <div class="record-label">PUNTAJE MÁS ALTO</div>
      <div class="record-score" id="record-display">0</div>
    </div>
    <div id="record-history" style="width:100%;max-height:200px;overflow-y:auto;margin-bottom:20px;"></div>
    <button class="btn btn-secondary" onclick="closeModal('modal-record')">✖ CERRAR</button>
  </div>
</div>

<!-- ========== TOAST ========== -->
<div class="toast" id="toast">Mensaje</div>

<script>
// ============================================================
//  STUDI-BOT  –  Engine + Data
// ============================================================

// -------- STATE --------
let state = {
  score: 0,
  lives: 3,
  ram: 30,
  record: 0,
  recordHistory: [],
  currentWorld: 0,
  currentLevelIndex: 0,
  levelOrder: [],
  soundOn: true,
  volume: 0.6,
  paused: false,
  hintRevealed: false,
  answered: false,
  timerInterval: null,
  timerSeconds: 30,
  timerMax: 30,
  worldCompleted: [false, false, false, false]
};

// -------- AUDIO --------
let audioCtx = null;
function getAudioCtx() {
  if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  return audioCtx;
}
function playTone(freq, type, dur, vol) {
  if (!state.soundOn) return;
  try {
    const ctx = getAudioCtx();
    const o = ctx.createOscillator();
    const g = ctx.createGain();
    o.connect(g); g.connect(ctx.destination);
    o.type = type || 'sine';
    o.frequency.setValueAtTime(freq, ctx.currentTime);
    g.gain.setValueAtTime((vol || 0.3) * state.volume, ctx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + dur);
    o.start(ctx.currentTime);
    o.stop(ctx.currentTime + dur);
  } catch(e) {}
}
function soundCorrect() {
  playTone(523, 'sine', 0.12, 0.4);
  setTimeout(() => playTone(659, 'sine', 0.12, 0.4), 100);
  setTimeout(() => playTone(784, 'sine', 0.2, 0.4), 200);
}
function soundWrong() {
  playTone(300, 'sawtooth', 0.1, 0.3);
  setTimeout(() => playTone(250, 'sawtooth', 0.2, 0.3), 120);
}
function soundSelect() { playTone(440, 'sine', 0.08, 0.2); }
function soundStart() {
  [261,329,392,523].forEach((f,i) => setTimeout(() => playTone(f,'triangle',0.15,0.3), i*80));
}
function soundAlert() { playTone(880, 'square', 0.1, 0.25); }
function setVolume(v) { state.volume = v / 100; }

// -------- BG CANVAS --------
(function initBG() {
  const c = document.getElementById('bg-canvas');
  const ctx = c.getContext('2d');
  let W, H, nodes = [];
  function resize() {
    W = c.width = window.innerWidth;
    H = c.height = window.innerHeight;
  }
  resize();
  window.addEventListener('resize', resize);
  const icons = ['💻','🖥','📡','🔌','🛜','🖨','⌨','🖱','💾','📟','🔧','⚙','🔲'];
  for (let i = 0; i < 22; i++) {
    nodes.push({
      x: Math.random() * 1400, y: Math.random() * 900,
      vx: (Math.random()-0.5)*0.3, vy: (Math.random()-0.5)*0.3,
      icon: icons[Math.floor(Math.random()*icons.length)],
      size: 14 + Math.random()*12,
      alpha: 0.3 + Math.random()*0.4
    });
  }
  function draw() {
    ctx.clearRect(0,0,W,H);
    // lines between close nodes
    for (let i = 0; i < nodes.length; i++) {
      for (let j = i+1; j < nodes.length; j++) {
        const dx = nodes[i].x - nodes[j].x;
        const dy = nodes[i].y - nodes[j].y;
        const d = Math.sqrt(dx*dx+dy*dy);
        if (d < 160) {
          ctx.beginPath();
          ctx.strokeStyle = `rgba(0,229,255,${0.15*(1-d/160)})`;
          ctx.lineWidth = 0.6;
          ctx.moveTo(nodes[i].x, nodes[i].y);
          ctx.lineTo(nodes[j].x, nodes[j].y);
          ctx.stroke();
        }
      }
    }
    nodes.forEach(n => {
      ctx.save();
      ctx.globalAlpha = n.alpha * 0.6;
      ctx.font = n.size+'px serif';
      ctx.fillText(n.icon, n.x, n.y);
      ctx.restore();
      n.x += n.vx; n.y += n.vy;
      if (n.x < -30) n.x = W + 30;
      if (n.x > W + 30) n.x = -30;
      if (n.y < -30) n.y = H + 30;
      if (n.y > H + 30) n.y = -30;
    });
    requestAnimationFrame(draw);
  }
  draw();
})();

// -------- CONFETTI --------
function launchConfetti() {
  const canvas = document.getElementById('confetti-canvas');
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  const pieces = [];
  const colors = ['#00e5ff','#42a5f5','#ffd600','#00e676','#ff6b6b','#ffffff'];
  for (let i = 0; i < 90; i++) {
    pieces.push({
      x: Math.random() * canvas.width,
      y: -10, vy: 2 + Math.random()*4, vx: (Math.random()-0.5)*3,
      size: 6 + Math.random()*8,
      color: colors[Math.floor(Math.random()*colors.length)],
      rot: Math.random()*360, rspeed: (Math.random()-0.5)*5,
      alpha: 1
    });
  }
  let frames = 0;
  function loop() {
    ctx.clearRect(0,0,canvas.width,canvas.height);
    pieces.forEach(p => {
      ctx.save();
      ctx.translate(p.x, p.y);
      ctx.rotate(p.rot * Math.PI/180);
      ctx.globalAlpha = p.alpha;
      ctx.fillStyle = p.color;
      ctx.fillRect(-p.size/2, -p.size/2, p.size, p.size*0.5);
      ctx.restore();
      p.x += p.vx; p.y += p.vy;
      p.rot += p.rspeed;
      if (frames > 60) p.alpha -= 0.018;
    });
    frames++;
    if (frames < 130) requestAnimationFrame(loop);
    else ctx.clearRect(0,0,canvas.width,canvas.height);
  }
  loop();
}

// -------- TOAST --------
let toastTimer;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 2500);
}

// -------- WORLD DATA --------
const worlds = [
  {
    id: 0,
    name: "Conocimiento General",
    icon: "🖥",
    color: "#00e5ff",
    subtitle: "Fundamentos del Hardware",
    description: "Bases del computador, sus partes y funciones esenciales.",
    keyIdea: "Un computador es un sistema integrado de hardware y software. El hardware incluye todos los componentes físicos que permiten procesar, almacenar y comunicar información.",
    concepts: [
      { icon: "🧠", term: "CPU", def: "Unidad central de procesamiento. El 'cerebro' del computador." },
      { icon: "💾", term: "RAM", def: "Memoria de acceso aleatorio. Almacenamiento temporal de datos activos." },
      { icon: "💿", term: "Almacenamiento", def: "HDD/SSD: guarda datos de forma permanente." },
      { icon: "🖥", term: "Periféricos", def: "Dispositivos de entrada y salida: teclado, mouse, monitor." },
      { icon: "⚡", term: "Fuente poder", def: "Convierte corriente alterna en continua para el sistema." },
      { icon: "🔌", term: "Placa madre", def: "Tarjeta principal que conecta todos los componentes." }
    ]
  },
  {
    id: 1,
    name: "Interesado en el Medio",
    icon: "📡",
    color: "#42a5f5",
    subtitle: "Redes y Conectividad",
    description: "Dispositivos de red, topologías y conceptos de conectividad.",
    keyIdea: "Una red de computadores conecta dispositivos para compartir recursos e información. Los protocolos, topologías y dispositivos de red son la base de la comunicación digital moderna.",
    concepts: [
      { icon: "🔀", term: "Switch", def: "Conecta dispositivos en una red LAN de forma eficiente." },
      { icon: "🌐", term: "Router", def: "Dirige el tráfico entre redes distintas (LAN/WAN)." },
      { icon: "📶", term: "Access Point", def: "Punto de acceso inalámbrico a la red." },
      { icon: "🔗", term: "Topología", def: "Estructura lógica/física de la red (estrella, bus, anillo)." },
      { icon: "📋", term: "IP Address", def: "Dirección numérica que identifica un dispositivo en la red." },
      { icon: "🛡", term: "Firewall", def: "Controla el tráfico de red para proteger la seguridad." }
    ]
  },
  {
    id: 2,
    name: "Instruido en el Medio",
    icon: "⚙",
    color: "#00bcd4",
    subtitle: "Mantenimiento y Diagnóstico",
    description: "Mantenimiento preventivo, diagnóstico de fallas y soporte técnico.",
    keyIdea: "El mantenimiento preventivo y el diagnóstico técnico son claves para garantizar la vida útil del hardware. Identificar síntomas y aplicar soluciones correctas evita pérdidas de datos y tiempo.",
    concepts: [
      { icon: "🔧", term: "Mantenimiento", def: "Preventivo vs correctivo: limpieza, reemplazo, actualización." },
      { icon: "🌡", term: "Temperatura", def: "El sobrecalentamiento es la principal causa de fallas." },
      { icon: "📊", term: "Diagnóstico", def: "Herramientas para identificar y resolver fallos del sistema." },
      { icon: "💡", term: "BIOS/UEFI", def: "Firmware que inicializa hardware antes del sistema operativo." },
      { icon: "🔋", term: "POST", def: "Power-On Self Test: prueba automática al encender el equipo." },
      { icon: "📦", term: "Drivers", def: "Controladores que permiten al SO comunicarse con el hardware." }
    ]
  },
  {
    id: 3,
    name: "Técnico",
    icon: "🔬",
    color: "#0097a7",
    subtitle: "Nivel Avanzado y Redes",
    description: "Direccionamiento IP, protocolos avanzados, arquitectura y seguridad.",
    keyIdea: "El nivel técnico integra conocimientos de arquitectura de sistemas, protocolos de red avanzados, subnetting, seguridad informática y administración de infraestructura tecnológica.",
    concepts: [
      { icon: "🔢", term: "Subnetting", def: "División de redes IP en subredes para mayor eficiencia." },
      { icon: "📡", term: "OSI Model", def: "7 capas que estandarizan la comunicación en redes." },
      { icon: "🔒", term: "Criptografía", def: "Técnicas para proteger la confidencialidad de los datos." },
      { icon: "⚡", term: "RAID", def: "Arreglo de discos para redundancia y rendimiento." },
      { icon: "🖧", term: "VLAN", def: "Red virtual dentro de la red física para segmentar tráfico." },
      { icon: "📜", term: "TCP/IP", def: "Suite de protocolos base de Internet y redes modernas." }
    ]
  }
];

// -------- QUESTION BANKS --------
const questionBanks = [

// WORLD 0 - Conocimiento General
[
  { mission:"Identifica los componentes", fragment:"El computador tiene componentes internos y externos. La CPU procesa instrucciones, la RAM guarda datos temporales y el disco duro almacena datos permanentes.", hint:"Piensa en qué hace cada componente.", question:"¿Cuál componente se llama el 'cerebro' del computador?", options:["Disco duro","CPU","RAM","Monitor"], correct:1, explanation:"La CPU (Unidad Central de Procesamiento) ejecuta todas las instrucciones del sistema operativo y los programas. Es el componente más importante del procesamiento.", remember:"CPU = Central Processing Unit = cerebro digital.", goal:"Identificar la función de la CPU" },
  { mission:"Función de la memoria", fragment:"La RAM (Random Access Memory) almacena temporalmente los datos que el procesador necesita de forma inmediata. Al apagar el equipo, su contenido se borra.", hint:"Es temporal y volátil.", question:"¿Qué ocurre con la información en la RAM al apagar el computador?", options:["Se guarda automáticamente","Se borra completamente","Se transfiere al disco","Se comprime"], correct:1, explanation:"La RAM es una memoria volátil: necesita energía eléctrica constante para mantener los datos. Al apagar el equipo, toda la información se pierde.", remember:"RAM = volátil. SSD/HDD = persistente.", goal:"Comprender la volatilidad de la RAM" },
  { mission:"Tipos de almacenamiento", fragment:"Los dispositivos de almacenamiento incluyen HDD (disco duro mecánico) y SSD (estado sólido). El SSD es más rápido y resistente a golpes.", hint:"SSD no tiene partes mecánicas.", question:"¿Cuál es la principal ventaja del SSD sobre el HDD?", options:["Mayor capacidad","Menor precio","Mayor velocidad de lectura/escritura","Más compatible"], correct:2, explanation:"Los SSD usan memoria flash para leer y escribir datos mucho más rápido que los discos mecánicos HDD, lo que acelera el arranque del sistema y la carga de aplicaciones.", remember:"SSD = flash = rápido. HDD = disco giratorio = más económico por GB.", goal:"Comparar SSD y HDD" },
  { mission:"Periféricos de entrada", fragment:"Los periféricos de entrada permiten al usuario enviar información al computador. Los más comunes son el teclado y el ratón.", hint:"Envían datos al sistema.", question:"¿Cuál de estos es un periférico de ENTRADA?", options:["Monitor","Impresora","Teclado","Parlante"], correct:2, explanation:"El teclado envía pulsaciones al sistema operativo. Es un periférico de entrada porque transmite información desde el usuario al computador.", remember:"Entrada: teclado, mouse, micrófono, scanner. Salida: monitor, impresora, parlante.", goal:"Distinguir periféricos de entrada y salida" },
  { mission:"Periféricos de salida", fragment:"Los periféricos de salida presentan información procesada al usuario de forma visual, sonora o impresa.", hint:"Presentan resultados al usuario.", question:"¿Cuál de estos es exclusivamente un periférico de SALIDA?", options:["Cámara web","Monitor","Teclado táctil","Pendrive"], correct:1, explanation:"El monitor muestra la información generada por el procesador. No recibe datos del usuario directamente, solo los presenta.", remember:"Monitor = salida visual pura.", goal:"Clasificar correctamente los periféricos" },
  { mission:"La placa madre", fragment:"La placa madre (motherboard) es la tarjeta principal del computador. Conecta e interconecta todos los componentes: CPU, RAM, tarjetas de expansión y almacenamiento.", hint:"Es la 'autopista' que conecta todo.", question:"¿Cuál es la función principal de la placa madre?", options:["Almacenar datos del sistema","Procesar gráficos","Interconectar todos los componentes","Suministrar energía"], correct:2, explanation:"La motherboard actúa como columna vertebral del sistema, proporcionando ranuras, buses y conectores para que CPU, RAM, GPU y almacenamiento se comuniquen entre sí.", remember:"Motherboard = hub central de comunicación de hardware.", goal:"Entender el rol de la motherboard" },
  { mission:"Fuente de poder", fragment:"La fuente de poder (PSU) convierte la corriente alterna del tomacorriente en corriente continua de diferentes voltajes (3.3V, 5V, 12V) para los componentes.", hint:"Transforma y distribuye energía.", question:"¿Qué tipo de corriente usan los componentes internos del PC?", options:["Corriente alterna 110V","Corriente continua de bajo voltaje","Corriente alterna 220V","Corriente trifásica"], correct:1, explanation:"Los circuitos electrónicos del PC funcionan con corriente continua (DC) a bajos voltajes. La PSU transforma la CA doméstica en DC utilizable.", remember:"PSU convierte CA → CC. Sin ella, el hardware se quemaría.", goal:"Comprender la función de la fuente de poder" },
  { mission:"Tarjeta gráfica", fragment:"La GPU (Unidad de Procesamiento Gráfico) procesa y renderiza imágenes para el monitor. Puede ser integrada en la CPU o dedicada (tarjeta aparte).", hint:"Procesa pixels e imágenes.", question:"¿Qué componente procesa los gráficos para el monitor?", options:["RAM","CPU","GPU","Disco SSD"], correct:2, explanation:"La GPU está optimizada para realizar miles de cálculos paralelos para procesar gráficos. Es esencial para juegos, diseño y video en alta resolución.", remember:"GPU = Graphics Processing Unit. Integrada o dedicada.", goal:"Identificar la función de la GPU" },
  { mission:"Bus del sistema", fragment:"El bus del sistema es un conjunto de líneas eléctricas que permiten la comunicación entre CPU, RAM y otros componentes. Tiene diferentes anchos (8, 16, 32, 64 bits).", hint:"Piensa en una autopista de datos.", question:"¿Qué es el bus del sistema en un computador?", options:["Un protocolo de red","Un conjunto de cables para comunicar componentes","Un tipo de memoria","Un software de diagnóstico"], correct:1, explanation:"El bus es la infraestructura de comunicación interna. A mayor ancho de bus (bits), más datos pueden viajar simultáneamente entre componentes.", remember:"Bus = autopista interna de datos del PC.", goal:"Comprender el concepto de bus del sistema" },
  { mission:"Memoria caché", fragment:"La memoria caché es una memoria muy rápida y pequeña integrada en la CPU (niveles L1, L2, L3) que almacena datos frecuentemente usados para acelerar el procesamiento.", hint:"Está muy cerca de la CPU.", question:"¿Qué función cumple la memoria caché de la CPU?", options:["Reemplazar a la RAM","Almacenar el sistema operativo","Guardar datos de uso frecuente para acceso rápido","Hacer copias de seguridad"], correct:2, explanation:"La caché reduce el tiempo que la CPU espera datos de la RAM. Los niveles L1 (más rápido/pequeño) a L3 (más lento/grande) forman una jerarquía de velocidad.", remember:"Caché: L1 < L2 < L3 (tamaño) / L1 > L2 > L3 (velocidad).", goal:"Entender la jerarquía de memoria caché" },
  { mission:"Velocidad del procesador", fragment:"La velocidad de la CPU se mide en GHz (Gigahertz) y representa la cantidad de ciclos por segundo. Un procesador de 3.5 GHz realiza 3,500 millones de ciclos por segundo.", hint:"GHz = miles de millones de ciclos/seg.", question:"¿En qué unidad se mide la velocidad de un procesador?", options:["MB/s","GHz","Watts","GB"], correct:1, explanation:"Los GHz (Gigahertz) miden la frecuencia del reloj del procesador. Sin embargo, los núcleos, la arquitectura y el caché también afectan el rendimiento real.", remember:"Más GHz ≠ siempre más rápido. La arquitectura también importa.", goal:"Conocer la medición de velocidad del CPU" },
  { mission:"Registro del procesador", fragment:"Los registros son pequeñas unidades de almacenamiento dentro de la CPU. Son la memoria más rápida del sistema pero también la más pequeña (generalmente 64 bits).", hint:"Son los más pequeños y rápidos.", question:"¿Cuál es el tipo de memoria más rápida en la jerarquía de almacenamiento?", options:["SSD NVMe","RAM DDR5","Registros de la CPU","Caché L1"], correct:2, explanation:"Los registros están físicamente dentro de la CPU y almacenan datos en el ciclo actual de instrucción. Su velocidad es igual a la del procesador.", remember:"Jerarquía: Registros > Caché > RAM > SSD > HDD.", goal:"Ubicar los registros en la jerarquía de memoria" },
  { mission:"El BIOS", fragment:"El BIOS (Basic Input/Output System) es firmware almacenado en un chip de la motherboard que inicializa el hardware y carga el sistema operativo al encender el PC.", hint:"Es lo primero que ejecuta el PC.", question:"¿Qué hace el BIOS al encender el computador?", options:["Carga los drivers de video","Inicializa el hardware y arranca el SO","Formatea el disco duro","Actualiza los programas"], correct:1, explanation:"El BIOS ejecuta el POST (Power-On Self Test), detecta los dispositivos conectados y transfiere el control al gestor de arranque del sistema operativo.", remember:"BIOS → POST → Boot loader → Sistema Operativo.", goal:"Comprender el proceso de arranque del PC" },
  { mission:"Chipset de la placa madre", fragment:"El chipset es un conjunto de chips en la motherboard que coordina el flujo de datos entre la CPU, RAM, puertos USB, SATA y otros controladores.", hint:"Coordina el tráfico de datos interno.", question:"¿Cuál es la función del chipset en la placa madre?", options:["Generar gráficos","Coordinar la comunicación entre CPU y periféricos","Almacenar el sistema operativo","Regular la temperatura"], correct:1, explanation:"El chipset actúa como 'director de tráfico' de la motherboard. Gestiona la comunicación entre el procesador, la memoria y los dispositivos conectados.", remember:"Chipset = controlador de comunicaciones de la motherboard.", goal:"Entender el rol del chipset" },
  { mission:"Tipos de RAM", fragment:"Los tipos de RAM incluyen DDR3, DDR4 y DDR5. Cada generación ofrece mayor velocidad y menor consumo eléctrico. La compatibilidad depende de la motherboard.", hint:"DDR = Double Data Rate.", question:"¿Qué significa 'DDR' en las memorias RAM?", options:["Disco Duro Removible","Double Data Rate","Direct Drive RAM","Dual Digital Register"], correct:1, explanation:"DDR (Double Data Rate) significa que la memoria transfiere datos en el flanco de subida Y bajada del reloj, duplicando el ancho de banda respecto a las memorias SDR anteriores.", remember:"DDR5 > DDR4 > DDR3 en velocidad y eficiencia.", goal:"Identificar los tipos de RAM" },
  { mission:"Interfaces de almacenamiento", fragment:"Las interfaces de almacenamiento conectan los discos a la motherboard. Las principales son SATA (Serial ATA) y NVMe (a través de PCIe), siendo NVMe mucho más rápida.", hint:"NVMe usa la ranura PCIe.", question:"¿Cuál interfaz de almacenamiento ofrece mayor velocidad?", options:["SATA II","SATA III","NVMe PCIe","USB 2.0"], correct:2, explanation:"NVMe (Non-Volatile Memory Express) aprovecha el bus PCIe para velocidades de hasta 7,000 MB/s, frente a los ~600 MB/s máximos de SATA III.", remember:"NVMe > SATA III > SATA II en velocidad de transferencia.", goal:"Comparar interfaces de almacenamiento" },
  { mission:"Overclocking", fragment:"El overclocking es el proceso de aumentar la velocidad de reloj de la CPU o GPU más allá de las especificaciones del fabricante para obtener mayor rendimiento.", hint:"Aumenta la frecuencia del reloj.", question:"¿Cuál es el principal riesgo del overclocking?", options:["Mayor consumo de RAM","Sobrecalentamiento y daño del hardware","Pérdida de datos del disco","Incompatibilidad con software"], correct:1, explanation:"Al aumentar la frecuencia del reloj, el procesador genera más calor. Sin refrigeración adecuada, puede sobrecalentarse, reducir su vida útil o dañarse permanentemente.", remember:"Overclocking = más rendimiento, más calor, más riesgo.", goal:"Conocer los riesgos del overclocking" },
  { mission:"Ranuras de expansión", fragment:"Las ranuras de expansión en la motherboard (PCIe, AGP legacy) permiten agregar tarjetas adicionales como GPUs, tarjetas de red o capturadoras.", hint:"PCIe es el estándar actual.", question:"¿Qué tipo de ranura se usa para instalar una tarjeta gráfica moderna?", options:["ISA","AGP","PCI","PCIe x16"], correct:3, explanation:"PCIe (Peripheral Component Interconnect Express) x16 es el estándar actual para tarjetas gráficas. La versión 4.0 y 5.0 ofrecen altísimos anchos de banda.", remember:"PCIe x16 = ranura principal para GPU.", goal:"Identificar las ranuras de expansión" },
  { mission:"Sistemas de refrigeración", fragment:"Los sistemas de refrigeración evitan el sobrecalentamiento. Incluyen ventiladores (air cooling) y refrigeración líquida (AIO o custom loop), además de pasta térmica.", hint:"La pasta térmica mejora la transferencia de calor.", question:"¿Cuál es la función de la pasta térmica en el procesador?", options:["Aumentar la velocidad del CPU","Mejorar la transferencia de calor entre CPU y disipador","Lubricar el ventilador","Proteger contra la electricidad estática"], correct:1, explanation:"La pasta térmica rellena las microscópicas imperfecciones entre el IHS del CPU y el disipador, mejorando significativamente la conductividad térmica.", remember:"Sin pasta térmica, la CPU puede alcanzar temperaturas críticas.", goal:"Comprender la refrigeración del CPU" },
  { mission:"Consumo energético", fragment:"El consumo energético de un PC se mide en Watts (W). La fuente de poder debe ser capaz de suplir la demanda de todos los componentes con un margen de eficiencia del 80%+.", hint:"80 Plus = estándar de eficiencia.", question:"¿Qué significa la certificación '80 Plus' en una fuente de poder?", options:["Tiene 80 conectores","Tiene 80% o más de eficiencia energética","Soporta 80 componentes","Dura 80 años"], correct:1, explanation:"La certificación 80 Plus garantiza que la PSU convierte al menos el 80% de la energía AC en DC útil, desperdiciando máximo el 20% como calor.", remember:"80 Plus Bronze < Silver < Gold < Platinum < Titanium.", goal:"Entender la eficiencia de las fuentes de poder" },
  { mission:"Arquitectura de 64 bits", fragment:"Los procesadores de 64 bits pueden direccionar más de 4 GB de RAM (teóricamente hasta 16 exabytes) y manejar datos en bloques de 64 bits simultáneamente.", hint:"64 bits > 32 bits en capacidad de memoria.", question:"¿Cuánta RAM máxima puede usar un sistema operativo de 32 bits?", options:["8 GB","16 GB","4 GB","2 GB"], correct:2, explanation:"Un SO de 32 bits puede direccionar como máximo 2^32 = 4,294,967,296 bytes = 4 GB de RAM. Por esto los sistemas modernos son de 64 bits.", remember:"32 bits = máx 4 GB RAM. 64 bits = prácticamente ilimitado.", goal:"Comprender las arquitecturas de 32 y 64 bits" },
  { mission:"Puertos USB", fragment:"USB (Universal Serial Bus) es la interfaz estándar para conectar periféricos. Las versiones USB 2.0 (480 Mbps), USB 3.0 (5 Gbps) y USB 4 (40 Gbps) difieren en velocidad.", hint:"El color azul indica USB 3.0.", question:"¿Qué color identifica generalmente un puerto USB 3.0?", options:["Negro","Verde","Amarillo","Azul"], correct:3, explanation:"Los puertos USB 3.0 se distinguen por el color azul en su interior, mientras que USB 2.0 suele ser negro o blanco. USB 3.1 Gen 2 puede ser rojo.", remember:"Azul = USB 3.0 (SuperSpeed). Negro = USB 2.0 (Hi-Speed).", goal:"Identificar versiones USB por color" },
  { mission:"Memoria virtual", fragment:"La memoria virtual extiende la RAM usando espacio en disco (archivo de paginación/swap). Aunque es más lenta, permite ejecutar más programas simultáneamente.", hint:"Usa espacio del disco como 'RAM extra'.", question:"¿Qué es la memoria virtual en un sistema operativo?", options:["Una copia de la RAM en la nube","Espacio en disco usado como extensión de la RAM","Un tipo de RAM más rápida","La memoria de la GPU"], correct:1, explanation:"Cuando la RAM se llena, el SO mueve datos menos usados a un archivo de paginación en el disco. Esto es más lento pero permite ejecutar más aplicaciones.", remember:"Memoria virtual = RAM + espacio de swap en disco.", goal:"Entender la memoria virtual" },
  { mission:"Formato de factores de forma", fragment:"El factor de forma define el tamaño y layout de la placa madre. Los estándares son ATX (full), MicroATX y Mini-ITX, determinando qué carcasa y fuente usar.", hint:"ATX es el más grande y el Mini-ITX el más pequeño.", question:"¿Cuál es el factor de forma más grande de los estándares comunes?", options:["Mini-ITX","MicroATX","ATX","Nano-ITX"], correct:2, explanation:"ATX (Advanced Technology eXtended) es el factor de forma estándar full-size con más ranuras de expansión, espacio para más RAM y mejores opciones de cooling.", remember:"ATX > MicroATX > Mini-ITX en tamaño y expansibilidad.", goal:"Conocer los factores de forma de motherboards" },
  { mission:"Tarjeta de sonido", fragment:"La tarjeta de sonido procesa señales de audio digitales y las convierte en señales analógicas para parlantes o audífonos. Puede ser integrada en la motherboard o dedicada.", hint:"Convierte señales digitales a analógicas.", question:"¿Cuál es la función principal de una tarjeta de sonido?", options:["Procesar gráficos de audio","Convertir señales digitales en analógicas para audio","Amplificar señales de red","Almacenar archivos de música"], correct:1, explanation:"La tarjeta de sonido realiza conversión DAC (Digital-Analog Converter) para reproducción de audio y ADC (Analog-Digital Converter) para grabación.", remember:"DAC: digital→analógico (reproducir). ADC: analógico→digital (grabar).", goal:"Entender la función de la tarjeta de sonido" },
  { mission:"Diagnóstico POST", fragment:"El POST (Power-On Self Test) es una prueba automática que verifica CPU, RAM, teclado y almacenamiento al encender. Los errores se comunican mediante pitidos o códigos.", hint:"Pitidos = códigos de error del POST.", question:"¿Qué indica un pitido largo seguido de dos cortos durante el arranque?", options:["El PC inició correctamente","Un error en la tarjeta de video","RAM insertada incorrectamente","Temperatura elevada"], correct:1, explanation:"Los códigos de pitido (beep codes) del BIOS indican errores específicos. 1 largo + 2 cortos generalmente señala problemas con la tarjeta gráfica (código AMI BIOS).", remember:"Los pitidos del POST son mensajes de error del BIOS.", goal:"Interpretar los códigos de pitido del POST" },
  { mission:"Unidad óptica", fragment:"Las unidades ópticas (CD/DVD/Blu-ray) leen y escriben datos usando láser. Su uso ha declinado con la popularización de USB y almacenamiento en la nube.", hint:"Usan láser para leer discos.", question:"¿Cuál es la capacidad aproximada de un disco Blu-ray de capa única?", options:["700 MB","4.7 GB","25 GB","100 GB"], correct:2, explanation:"Un Blu-ray de capa simple (SL) almacena 25 GB de datos, frente a los 4.7 GB de un DVD. Los discos Blu-ray de doble capa (DL) alcanzan 50 GB.", remember:"CD=700MB, DVD=4.7GB, Blu-ray SL=25GB.", goal:"Conocer las capacidades de medios ópticos" },
  { mission:"Memoria ROM", fragment:"La ROM (Read-Only Memory) es memoria de solo lectura que contiene instrucciones permanentes del fabricante. El BIOS/UEFI se almacena en ROM flash (EEPROM).", hint:"Contiene el firmware del sistema.", question:"¿Cuál es la principal diferencia entre ROM y RAM?", options:["La ROM es más rápida","La ROM es persistente y no volátil","La ROM tiene mayor capacidad","La ROM es programable por el usuario"], correct:1, explanation:"La ROM mantiene su contenido sin energía eléctrica (no volátil), mientras que la RAM pierde toda información al apagarse. La ROM almacena firmware esencial.", remember:"ROM = permanente. RAM = temporal.", goal:"Distinguir ROM y RAM" },
  { mission:"Puertos de video", fragment:"Los puertos de video conectan el computador al monitor. Los más comunes son HDMI (audio+video digital), DisplayPort (alta frecuencia), VGA (analógico) y DVI.", hint:"HDMI transmite audio y video.", question:"¿Cuál puerto de video puede transmitir tanto audio como video simultáneamente?", options:["VGA","DVI-D","DisplayPort","HDMI"], correct:3, explanation:"HDMI (High-Definition Multimedia Interface) transmite video de alta definición y audio multicanal por un solo cable, simplificando las conexiones.", remember:"HDMI = audio+video en un solo cable. VGA = solo video analógico.", goal:"Identificar los puertos de video" },
  { mission:"Tarjeta de red", fragment:"La tarjeta de red (NIC - Network Interface Card) conecta el computador a una red. Puede ser alámbrica (Ethernet) o inalámbrica (Wi-Fi) y se identifica por su dirección MAC.", hint:"MAC Address = identificador único de red.", question:"¿Qué es una dirección MAC?", options:["Un sistema operativo de Apple","La velocidad máxima de la red","El identificador único de la tarjeta de red","Un protocolo de seguridad"], correct:2, explanation:"La dirección MAC (Media Access Control) es un identificador único de 48 bits asignado por el fabricante a cada tarjeta de red. Es único a nivel mundial.", remember:"MAC = dirección física única de la NIC. Ej: 00:1A:2B:3C:4D:5E.", goal:"Conocer la dirección MAC" }
],

// WORLD 1 - Interesado en el Medio (Redes)
[
  { mission:"Identificar dispositivos de red", fragment:"Los dispositivos de red incluyen routers, switches, hubs, access points y modems. Cada uno tiene una función específica en la arquitectura de red.", hint:"Router conecta redes distintas.", question:"¿Cuál dispositivo conecta dos redes distintas, como LAN y WAN?", options:["Hub","Switch","Router","Bridge"], correct:2, explanation:"El router opera en la capa 3 (red) del modelo OSI y toma decisiones de enrutamiento basadas en direcciones IP para conectar redes diferentes.", remember:"Router = capa 3 = direcciones IP. Switch = capa 2 = direcciones MAC.", goal:"Identificar la función del router" },
  { mission:"Topología estrella", fragment:"En la topología estrella, todos los dispositivos se conectan a un nodo central (switch o hub). Si el nodo falla, toda la red cae, pero los fallos en nodos extremos son aislados.", hint:"El switch es el nodo central.", question:"¿Cuál es la principal desventaja de la topología estrella?", options:["Difícil de instalar","Si el nodo central falla, cae toda la red","Es muy lenta","No soporta más de 5 nodos"], correct:1, explanation:"La topología estrella depende completamente del nodo central. Si el switch o hub central falla, todos los dispositivos pierden conectividad.", remember:"Estrella: fácil de gestionar, punto único de fallo en el centro.", goal:"Comprender la topología estrella" },
  { mission:"Protocolo IP", fragment:"IP (Internet Protocol) asigna direcciones numéricas a dispositivos en red. IPv4 usa 32 bits (4 grupos de números del 0 al 255). IPv6 usa 128 bits.", hint:"IPv4: cuatro grupos de números.", question:"¿Cuántos bits tiene una dirección IPv4?", options:["16 bits","64 bits","32 bits","128 bits"], correct:2, explanation:"IPv4 usa 32 bits divididos en 4 octetos (grupos de 8 bits), dando 4,294,967,296 direcciones posibles. Ej: 192.168.1.100", remember:"IPv4 = 32 bits = ~4,300 millones de direcciones. IPv6 = 128 bits.", goal:"Conocer la estructura del protocolo IP" },
  { mission:"Switch vs Hub", fragment:"El hub retransmite los datos a todos los puertos (broadcast). El switch aprende las direcciones MAC y envía los datos solo al puerto destino correcto (unicast).", hint:"El switch es más inteligente que el hub.", question:"¿Por qué el switch es más eficiente que el hub?", options:["Porque es más barato","Porque envía datos solo al destino correcto","Porque usa IP en vez de MAC","Porque opera más rápido eléctricamente"], correct:1, explanation:"El switch mantiene una tabla MAC y dirige cada trama solo al puerto correcto, evitando colisiones y reduciendo el tráfico innecesario en la red.", remember:"Hub = tonto (broadcast). Switch = inteligente (unicast por MAC).", goal:"Comparar hub y switch" },
  { mission:"Modelo OSI", fragment:"El modelo OSI tiene 7 capas: Física, Enlace de datos, Red, Transporte, Sesión, Presentación y Aplicación. Cada capa tiene funciones específicas.", hint:"Recuerda: 'Por Favor, No Tires Sobre Pizzas Aun'.", question:"¿Cuántas capas tiene el modelo OSI?", options:["4","5","6","7"], correct:3, explanation:"El modelo OSI (Open Systems Interconnection) tiene 7 capas que estandarizan la comunicación en redes. Cada capa se comunica con la inmediatamente superior e inferior.", remember:"OSI: Física→Enlace→Red→Transporte→Sesión→Presentación→Aplicación.", goal:"Memorizar las capas del modelo OSI" },
  { mission:"Dirección IP privada", fragment:"Las direcciones IP privadas (192.168.x.x, 10.x.x.x, 172.16-31.x.x) son usadas en redes locales y no son enrutables en Internet.", hint:"Rangos privados definidos en RFC 1918.", question:"¿Cuál de estas es una dirección IP privada?", options:["8.8.8.8","172.217.14.110","192.168.1.50","203.0.113.5"], correct:2, explanation:"192.168.1.50 pertenece al rango 192.168.0.0/16, definido como privado en RFC 1918. No es enrutable en Internet sin NAT.", remember:"Privadas: 10.x, 172.16-31.x, 192.168.x — no van a Internet.", goal:"Identificar direcciones IP privadas" },
  { mission:"Protocolo DHCP", fragment:"DHCP (Dynamic Host Configuration Protocol) asigna automáticamente direcciones IP, máscara de subred, puerta de enlace y DNS a los dispositivos al conectarse a la red.", hint:"Asigna IP de forma automática.", question:"¿Qué protocolo asigna automáticamente direcciones IP en una red?", options:["DNS","FTP","DHCP","HTTP"], correct:2, explanation:"El servidor DHCP mantiene un pool de IPs disponibles y asigna una a cada dispositivo que la solicita, evitando configuración manual.", remember:"DHCP = asignación automática de IP. Sin DHCP = configuración manual.", goal:"Comprender la función del DHCP" },
  { mission:"Topología bus", fragment:"En la topología bus, todos los dispositivos comparten un único cable (bus). Las colisiones son frecuentes y si el cable principal se daña, toda la red falla.", hint:"Un solo cable para todos.", question:"¿Cuál es el principal problema de la topología de bus?", options:["No soporta más de 2 dispositivos","El cable central es punto único de fallo","Es muy costosa","Solo funciona con fiber óptica"], correct:1, explanation:"En la topología bus, un solo cable comparte todos los datos. Si el cable troncal se corta, toda la red deja de funcionar. Además, las colisiones degradan el rendimiento.", remember:"Bus: económica pero frágil y propensa a colisiones.", goal:"Identificar limitaciones de la topología bus" },
  { mission:"Cable de red", fragment:"Los cables de par trenzado (UTP/STP) son los más usados en redes LAN. Las categorías Cat5e, Cat6 y Cat6a definen la velocidad y ancho de banda máximos.", hint:"Cat6 soporta Gigabit Ethernet.", question:"¿Cuál es la velocidad máxima de un cable Cat6 en redes Gigabit?", options:["100 Mbps","1 Gbps","10 Gbps a 55m","1 Gbps o 10 Gbps a corta distancia"], correct:3, explanation:"Cat6 soporta 1 Gbps a 100m y 10 Gbps hasta 55m. Cat6a mejora esto a 10 Gbps en 100m. Ambos son estándar en instalaciones modernas.", remember:"Cat5e=1Gbps/100m. Cat6=1Gbps/100m o 10Gbps/55m. Cat6a=10Gbps/100m.", goal:"Conocer las categorías de cable de red" },
  { mission:"Firewall", fragment:"Un firewall es un sistema (hardware o software) que filtra el tráfico de red basándose en reglas de seguridad para proteger la red interna de amenazas externas.", hint:"Controla qué tráfico entra y sale.", question:"¿En qué capa del modelo OSI opera principalmente un firewall de inspección de estado?", options:["Capa 1 (Física)","Capas 3 y 4 (Red y Transporte)","Solo capa 7 (Aplicación)","Capa 2 (Enlace)"], correct:1, explanation:"Los firewalls de inspección de estado trabajan en capas 3 (IP) y 4 (TCP/UDP), analizando cabeceras de paquetes y estados de conexión.", remember:"Firewall = filtro de tráfico. Opera en capas 3-4 principalmente.", goal:"Entender cómo funciona un firewall" },
  { mission:"Protocolo DNS", fragment:"DNS (Domain Name System) traduce nombres de dominio (www.ejemplo.com) a direcciones IP numéricas. Sin DNS, habría que memorizar IPs para acceder a sitios web.", hint:"Traduce nombres a IPs.", question:"¿Cuál es la función del protocolo DNS?", options:["Asignar IPs automáticamente","Cifrar el tráfico web","Traducir nombres de dominio a IPs","Gestionar contraseñas web"], correct:2, explanation:"El DNS funciona como una agenda telefónica de Internet. Cuando escribes una URL, el DNS resuelve el nombre al número IP del servidor correspondiente.", remember:"DNS = agenda de Internet. Nombre → IP.", goal:"Comprender el protocolo DNS" },
  { mission:"Ancho de banda", fragment:"El ancho de banda es la cantidad máxima de datos que puede transmitir un enlace de red por segundo. Se mide en bps (bits por segundo), Mbps o Gbps.", hint:"Es la 'autopista' de datos.", question:"¿Cómo se denomina la cantidad máxima de datos que puede transmitir una red por segundo?", options:["Latencia","Ancho de banda","Throughput","Jitter"], correct:1, explanation:"El ancho de banda define la capacidad teórica del enlace. El throughput es el ancho de banda real obtenido considerando overhead y pérdidas.", remember:"Ancho de banda = capacidad. Latencia = retraso. Throughput = rendimiento real.", goal:"Diferenciar ancho de banda de otros parámetros" },
  { mission:"Protocolo TCP", fragment:"TCP (Transmission Control Protocol) garantiza la entrega ordenada y sin errores de datos mediante confirmaciones (ACK), retransmisiones y control de flujo.", hint:"TCP garantiza la entrega.", question:"¿Qué diferencia principal tiene TCP sobre UDP?", options:["TCP es más rápido","TCP garantiza la entrega de datos","TCP usa menos recursos","TCP es solo para video"], correct:1, explanation:"TCP establece una conexión (handshake 3 vías), verifica cada segmento con ACK y retransmite si hay pérdidas. UDP no tiene estas garantías pero es más rápido.", remember:"TCP = fiable, ordenado, orientado a conexión. UDP = rápido, no fiable.", goal:"Comparar TCP y UDP" },
  { mission:"Máscara de subred", fragment:"La máscara de subred define qué parte de la IP identifica la red y qué parte identifica el host. Ej: 255.255.255.0 significa que los primeros 3 octetos son la red.", hint:"Ej: /24 = 255.255.255.0", question:"¿Cuántos hosts admite una subred /24 (255.255.255.0)?", options:["256","254","128","512"], correct:1, explanation:"Una subred /24 tiene 256 direcciones (2^8), pero 2 se reservan: la dirección de red (.0) y la de broadcast (.255). Disponibles: 254 hosts.", remember:"/24 = 256 total, 254 usables. La fórmula es 2^(bits host) - 2.", goal:"Calcular hosts disponibles en una subred" },
  { mission:"Wi-Fi standards", fragment:"Los estándares Wi-Fi evolucionaron de 802.11b (11Mbps) a 802.11ax (Wi-Fi 6, hasta 9.6Gbps). Cada generación mejora velocidad, alcance y eficiencia espectral.", hint:"Wi-Fi 6 = 802.11ax.", question:"¿Cuál es el nombre de marketing del estándar IEEE 802.11ax?", options:["Wi-Fi 5","Wi-Fi 6","Wi-Fi 4","Wi-Fi 7"], correct:1, explanation:"802.11ax fue renombrado Wi-Fi 6 (y Wi-Fi 6E para la banda de 6 GHz). Ofrece mayor eficiencia en entornos con muchos dispositivos (OFDMA, MU-MIMO mejorado).", remember:"802.11ac = Wi-Fi 5. 802.11ax = Wi-Fi 6. 802.11be = Wi-Fi 7.", goal:"Conocer los estándares Wi-Fi" },
  { mission:"VLAN", fragment:"Una VLAN (Virtual LAN) segmenta lógicamente una red física en múltiples redes virtuales independientes, mejorando la seguridad y reduciendo el tráfico de broadcast.", hint:"Separa redes dentro del mismo switch.", question:"¿Cuál es el principal beneficio de implementar VLANs?", options:["Aumentar la velocidad del switch","Segmentar el tráfico y mejorar la seguridad","Reemplazar al router","Eliminar la necesidad de IPs"], correct:1, explanation:"Las VLANs permiten que departamentos diferentes (RRHH, TI, Producción) tengan sus propios dominios de broadcast, aumentando seguridad e independencia.", remember:"VLAN = segmentación lógica sin hardware adicional.", goal:"Entender el propósito de las VLANs" },
  { mission:"NAT", fragment:"NAT (Network Address Translation) traduce direcciones IP privadas a una pública para acceder a Internet y viceversa. Permite que múltiples dispositivos compartan una IP pública.", hint:"Múltiples PCs, una sola IP pública.", question:"¿Qué problema resuelve principalmente el NAT?", options:["La velocidad de la red","El agotamiento de direcciones IPv4 públicas","La seguridad en redes inalámbricas","Los conflictos de DNS"], correct:1, explanation:"Con NAT, toda una red privada puede salir a Internet con una sola IP pública. Esto fue clave para extender la vida útil del espacio IPv4.", remember:"NAT: IP privada ↔ IP pública. Solución temporal al agotamiento de IPv4.", goal:"Comprender la función del NAT" },
  { mission:"Protocolo ICMP", fragment:"ICMP (Internet Control Message Protocol) se usa para diagnóstico de red. El comando 'ping' usa ICMP para verificar la conectividad entre dos hosts.", hint:"Ping usa ICMP.", question:"¿Qué protocolo utiliza el comando 'ping'?", options:["TCP","UDP","ICMP","ARP"], correct:2, explanation:"Ping envía mensajes ICMP Echo Request y espera Echo Reply para medir latencia y verificar alcanzabilidad entre dispositivos en la red.", remember:"ping = ICMP Echo Request/Reply. Mide latencia y verifica conectividad.", goal:"Conocer el protocolo ICMP y el comando ping" },
  { mission:"Bandwidth vs Latencia", fragment:"El ancho de banda es la capacidad de la red. La latencia es el tiempo de ida y vuelta de un paquete (RTT). Ambos afectan la experiencia del usuario de distinta forma.", hint:"Latencia = tiempo de respuesta.", question:"¿Qué parámetro mide el tiempo que tarda un paquete en ir y volver?", options:["Ancho de banda","Jitter","RTT (Round Trip Time)","Throughput"], correct:2, explanation:"RTT (Round Trip Time) es el tiempo total que tarda un paquete en ir desde el origen, llegar al destino y regresar. Se mide con el comando ping.", remember:"RTT = tiempo ida + vuelta. Bajo RTT = baja latencia = buena respuesta.", goal:"Medir y comprender la latencia en redes" },
  { mission:"Topología malla", fragment:"En la topología de malla completa, cada nodo está conectado a todos los demás. Ofrece máxima redundancia pero es muy costosa en cables y configuración.", hint:"Fórmula: n(n-1)/2 conexiones.", question:"¿Cuántas conexiones necesita una topología malla completa de 5 nodos?", options:["5","8","10","15"], correct:2, explanation:"La fórmula es n(n-1)/2. Para 5 nodos: 5×4/2 = 10 conexiones. Esta redundancia garantiza que la red funcione aunque fallen múltiples enlaces.", remember:"Malla completa: n(n-1)/2 enlaces. Máxima redundancia, máximo costo.", goal:"Calcular conexiones en topología malla" },
  { mission:"Protocolo ARP", fragment:"ARP (Address Resolution Protocol) resuelve direcciones IP a direcciones MAC dentro de una red local, permitiendo que el switch entregue las tramas al destino correcto.", hint:"IP → MAC en la LAN.", question:"¿Qué protocolo resuelve una dirección IP a su correspondiente dirección MAC?", options:["DNS","DHCP","ARP","RARP"], correct:2, explanation:"Cuando un host quiere comunicarse con otro en la misma LAN, usa ARP para preguntar '¿quién tiene esta IP?' y obtener la MAC correspondiente.", remember:"ARP: IP→MAC (local). DNS: nombre→IP (global). Inverso: RARP.", goal:"Comprender el protocolo ARP" },
  { mission:"Capa de Transporte", fragment:"La capa 4 del modelo OSI (Transporte) gestiona la comunicación extremo a extremo. Los protocolos TCP (fiable) y UDP (no fiable) operan en esta capa.", hint:"TCP y UDP son de la capa 4.", question:"¿En qué capa del modelo OSI operan TCP y UDP?", options:["Capa 3 (Red)","Capa 2 (Enlace)","Capa 4 (Transporte)","Capa 5 (Sesión)"], correct:2, explanation:"La capa de transporte proporciona comunicación lógica entre procesos de aplicación usando puertos. TCP garantiza entrega; UDP prioriza velocidad.", remember:"Capa 4 = Transporte = TCP + UDP = puertos.", goal:"Ubicar TCP/UDP en el modelo OSI" },
  { mission:"Cable de fibra óptica", fragment:"La fibra óptica transmite datos como pulsos de luz. Ofrece mayor ancho de banda y distancias que el cobre. Existen fibra monomodo (larga distancia) y multimodo (corta distancia).", hint:"Luz vs electricidad.", question:"¿Cuál es la principal ventaja de la fibra óptica sobre el cable de cobre?", options:["Menor costo de instalación","Mayor inmunidad a interferencias electromagnéticas y mayor distancia","Más fácil de empalmar","Mayor peso y resistencia"], correct:1, explanation:"La fibra óptica transmite luz, no electricidad, por lo que no sufre interferencias electromagnéticas (EMI) y puede cubrir distancias de kilómetros sin degradación.", remember:"Fibra: inmune a EMI, largas distancias, mayor ancho de banda. Desventaja: costo y fragilidad.", goal:"Comparar fibra óptica y cable de cobre" },
  { mission:"Puerto de red", fragment:"Los puertos de red (0-65535) identifican servicios específicos. Puertos conocidos: 80 (HTTP), 443 (HTTPS), 21 (FTP), 22 (SSH), 25 (SMTP).", hint:"80=HTTP, 443=HTTPS.", question:"¿Qué protocolo usa el puerto 443 por defecto?", options:["HTTP","FTP","HTTPS","SSH"], correct:2, explanation:"HTTPS (HTTP Secure) usa el puerto 443 y cifra las comunicaciones con TLS/SSL. Es el estándar para transferencias seguras en la web.", remember:"80=HTTP, 443=HTTPS, 22=SSH, 21=FTP, 25=SMTP, 53=DNS.", goal:"Identificar puertos de red comunes" },
  { mission:"Redundancia de red", fragment:"La redundancia de red garantiza disponibilidad continua mediante rutas alternativas. Protocolos como STP (Spanning Tree Protocol) evitan los bucles en redes redundantes.", hint:"STP evita bucles en switches.", question:"¿Qué protocolo evita los bucles en redes con switches redundantes?", options:["OSPF","STP (Spanning Tree Protocol)","RIP","VRRP"], correct:1, explanation:"STP desactiva automáticamente los enlaces redundantes para evitar tormentas de broadcast. RSTP (Rapid STP) es la versión mejorada más rápida.", remember:"STP/RSTP: elimina bucles en capa 2. Activa puertos bloqueados si falla el principal.", goal:"Entender el protocolo STP" },
  { mission:"Modelo TCP/IP", fragment:"El modelo TCP/IP tiene 4 capas: Acceso a la red, Internet, Transporte y Aplicación. Es el modelo práctico en que funciona Internet.", hint:"4 capas, no 7 como OSI.", question:"¿Cuántas capas tiene el modelo TCP/IP?", options:["7","5","4","3"], correct:2, explanation:"El modelo TCP/IP simplifica el OSI en 4 capas: Acceso a red (física+enlace), Internet (red), Transporte, y Aplicación (sesión+presentación+aplicación).", remember:"TCP/IP: Acceso→Internet→Transporte→Aplicación (4 capas).", goal:"Diferenciar modelo OSI y TCP/IP" },
  { mission:"Tráfico unicast, multicast, broadcast", fragment:"Unicast: un emisor a un receptor. Multicast: un emisor a un grupo. Broadcast: un emisor a todos en la red. Cada tipo tiene implicaciones en eficiencia.", hint:"Broadcast llega a todos.", question:"¿Qué tipo de tráfico llega a TODOS los dispositivos en una red local?", options:["Unicast","Multicast","Broadcast","Anycast"], correct:2, explanation:"El broadcast (ej: 192.168.1.255) se envía a todos los hosts del segmento de red. Si se abusa de él, puede saturar la red (tormenta de broadcast).", remember:"Broadcast = todos. Unicast = uno. Multicast = grupo.", goal:"Distinguir tipos de tráfico de red" },
  { mission:"QoS - Calidad de Servicio", fragment:"QoS (Quality of Service) prioriza ciertos tipos de tráfico de red (voz, video en tiempo real) sobre otros (descargas, correo) para garantizar experiencia de usuario.", hint:"Prioriza el tráfico crítico.", question:"¿Para qué se usa principalmente QoS en redes?", options:["Para aumentar el ancho de banda total","Para priorizar tráfico sensible a la latencia","Para bloquear sitios web","Para asignar IPs automáticamente"], correct:1, explanation:"QoS es esencial en redes que llevan voz IP (VoIP) y videoconferencias. Prioriza estos paquetes sobre las descargas, reduciendo cortes y retrasos.", remember:"QoS: voz y video primero. Sin QoS, los archivos compiten con llamadas.", goal:"Entender el propósito del QoS" },
  { mission:"Protocolo OSPF", fragment:"OSPF (Open Shortest Path First) es un protocolo de enrutamiento dinámico de estado de enlace que encuentra la ruta más corta usando el algoritmo de Dijkstra.", hint:"Calcula la ruta más corta.", question:"¿En qué tipo de protocolo se clasifica OSPF?", options:["Vector de distancia","Estado de enlace","Híbrido","Exterior (EGP)"], correct:1, explanation:"OSPF es un protocolo IGP de estado de enlace. Cada router conoce toda la topología y calcula rutas óptimas independientemente usando el algoritmo SPF.", remember:"OSPF = estado de enlace. RIP = vector distancia. EIGRP = híbrido.", goal:"Clasificar protocolos de enrutamiento dinámico" }
],

// WORLD 2 - Instruido en el Medio
[
  { mission:"Mantenimiento preventivo", fragment:"El mantenimiento preventivo incluye limpieza periódica de polvo, verificación de conexiones, actualización de drivers y monitoreo de temperatura para prevenir fallas.", hint:"Prevenir es mejor que corregir.", question:"¿Con qué frecuencia se recomienda hacer limpieza de polvo en un computador de escritorio?", options:["Cada semana","Cada 6 meses a 1 año","Solo cuando falla","Cada 5 años"], correct:1, explanation:"El polvo acumulado actúa como aislante térmico, elevando la temperatura de los componentes. La limpieza semestral o anual previene sobrecalentamiento y fallos prematuros.", remember:"Polvo = aislante = calor = fallo. Limpieza regular = vida útil prolongada.", goal:"Establecer plan de mantenimiento preventivo" },
  { mission:"Temperatura ideal del CPU", fragment:"Los procesadores modernos funcionan óptimamente entre 30°C y 70°C en carga. Temperaturas superiores a 90°C activan el throttling térmico y pueden dañar permanentemente el chip.", hint:"Throttling = reducción de velocidad por calor.", question:"¿Qué ocurre cuando un CPU supera su temperatura máxima?", options:["Se apaga el monitor","Activa el throttling térmico para reducir su velocidad","Aumenta automáticamente la frecuencia","Formatea el disco duro"], correct:1, explanation:"El throttling térmico reduce la velocidad del procesador para generar menos calor y protegerse. Si continúa sobrecalentándose, el sistema se apaga automáticamente.", remember:"Throttling = autoprotección del CPU. Solución: mejor refrigeración.", goal:"Gestionar temperatura del CPU" },
  { mission:"Herramientas de diagnóstico", fragment:"Herramientas como CrystalDiskInfo (disco), CPU-Z (procesador), HWMonitor (temperatura) y MemTest86 (RAM) permiten diagnosticar el estado del hardware.", hint:"Cada herramienta para cada componente.", question:"¿Qué herramienta se usa para diagnosticar el estado de salud de un disco duro?", options:["MemTest86","CPU-Z","CrystalDiskInfo / S.M.A.R.T.","HWMonitor"], correct:2, explanation:"S.M.A.R.T. (Self-Monitoring Analysis and Reporting Technology) es un sistema integrado en los discos que reporta indicadores de salud. CrystalDiskInfo lo visualiza.", remember:"S.M.A.R.T. = sistema de autodiagnóstico del disco. CrystalDiskInfo lo lee.", goal:"Usar herramientas de diagnóstico de hardware" },
  { mission:"Error de pantalla azul", fragment:"El BSOD (Blue Screen of Death) en Windows indica un error crítico del sistema. Los códigos de error (STOP codes) ayudan a identificar el componente o driver causante.", hint:"Los códigos de stop indican la causa.", question:"¿Qué significa BSOD en diagnóstico de Windows?", options:["Basic System Optimization Disk","Blue Screen Of Death (pantalla azul de la muerte)","Boot Sector Operating Diagnostic","Binary System Output Device"], correct:1, explanation:"El BSOD aparece cuando Windows detecta un error del que no puede recuperarse. Los códigos como 0x0000007E o MEMORY_MANAGEMENT indican la causa del fallo.", remember:"BSOD = error crítico. El código STOP indica el módulo causante.", goal:"Interpretar los códigos de error BSOD" },
  { mission:"Actualización de drivers", fragment:"Los drivers (controladores) deben actualizarse regularmente para corregir bugs, mejorar rendimiento y garantizar compatibilidad con el software. Los desactualizados causan inestabilidad.", hint:"Driver desactualizado = inestabilidad.", question:"¿Por qué es importante mantener los drivers actualizados?", options:["Para reducir el consumo de energía","Para corregir bugs y mejorar rendimiento y compatibilidad","Para aumentar la velocidad del disco","Para reducir el tamaño del sistema"], correct:1, explanation:"Los fabricantes lanzan nuevos drivers para corregir vulnerabilidades, mejorar compatibilidad con SO actualizados y optimizar el rendimiento del hardware.", remember:"Drivers: gráficos, red, audio, chipset. Actualizar regularmente.", goal:"Gestionar la actualización de drivers" },
  { mission:"Diagnóstico de RAM", fragment:"Problemas de RAM se manifiestan como pantallas azules, reinicios aleatorios o errores en aplicaciones. MemTest86 realiza pruebas exhaustivas de la memoria.", hint:"MemTest86 se ejecuta desde USB.", question:"¿Cuál es el síntoma más común de una RAM defectuosa?", options:["El PC no muestra video","Pantallas azules y reinicios aleatorios","El disco hace ruido","El teclado no responde"], correct:1, explanation:"Una RAM defectuosa provoca errores aleatorios en la escritura/lectura de datos, causando BSODs, congelamientos y reinicios impredecibles.", remember:"RAM mala = BSOD y reinicios. Prueba: MemTest86, mínimo 2 pasadas.", goal:"Diagnosticar problemas de RAM" },
  { mission:"CHKDSK y escaneo de disco", fragment:"CHKDSK (Check Disk) es una utilidad de Windows que verifica y repara errores en el sistema de archivos y sectores defectuosos del disco.", hint:"chkdsk /f /r en cmd.", question:"¿Qué hace el comando 'chkdsk /f' en Windows?", options:["Formatea el disco","Repara errores en el sistema de archivos","Desfragmenta el disco","Elimina archivos temporales"], correct:1, explanation:"El parámetro /f hace que CHKDSK repare los errores encontrados. El /r además busca y recupera información de sectores defectuosos.", remember:"chkdsk /f = repara sistema archivos. chkdsk /r = también recupera sectores.", goal:"Usar CHKDSK para diagnóstico de disco" },
  { mission:"Copia de seguridad", fragment:"Los backups (copias de seguridad) protegen contra pérdida de datos por hardware defectuoso, ransomware o error humano. La regla 3-2-1: 3 copias, 2 medios, 1 externo.", hint:"Regla 3-2-1 para backups.", question:"¿Qué establece la regla 3-2-1 en gestión de backups?", options:["3 sistemas, 2 backups, 1 formato","3 copias, en 2 tipos de medios, con 1 copia fuera del sitio","3 backups diarios, 2 semanales, 1 mensual","3 archivos, 2 carpetas, 1 disco"], correct:1, explanation:"La regla 3-2-1 es la mejor práctica: 3 copias del dato, en 2 tipos de medios diferentes, con 1 copia en ubicación remota. Protege contra múltiples tipos de fallos.", remember:"3-2-1: tres copias, dos medios, uno offsite.", goal:"Implementar estrategia de backup 3-2-1" },
  { mission:"Pasta térmica avanzada", fragment:"La pasta térmica existe en diferentes tipos: zinc (básica), base plata (buena conductividad) y metal líquido (excelente pero riesgo de cortocircuito). La aplicación correcta es crucial.", hint:"Metal líquido = máximo rendimiento pero riesgo.", question:"¿Cuál es el riesgo principal al usar pasta térmica de metal líquido?", options:["Mayor temperatura","Menor durabilidad","Posible cortocircuito si entra en contacto con componentes","Incompatibilidad con AMD"], correct:2, explanation:"El metal líquido (generalmente aleación de Galio) es conductor eléctrico. Si se derrama sobre la placa madre, puede causar cortocircuitos y dañar permanentemente el sistema.", remember:"Metal líquido = mejor termoconductividad pero electricamente conductor = riesgo.", goal:"Seleccionar el tipo correcto de pasta térmica" },
  { mission:"Reemplazo de disco duro", fragment:"Al reemplazar un HDD por un SSD, se puede clonar el disco o realizar instalación limpia. La clonación conserva el sistema operativo y datos; la instalación limpia parte de cero.", hint:"Clonación vs instalación limpia.", question:"¿Cuál es la ventaja de clonar el disco al migrar de HDD a SSD?", options:["Mayor velocidad final","Conservar el SO y todos los datos sin reinstalar","Eliminar archivos corruptos","Mayor capacidad de almacenamiento"], correct:1, explanation:"La clonación copia el disco sector a sector, manteniendo el sistema operativo, programas y archivos. Solo requiere intercambiar el disco físicamente.", remember:"Clonación: conserva todo. Instalación limpia: más rápido/limpio pero reinstalar todo.", goal:"Planificar migración de HDD a SSD" },
  { mission:"Electricidad estática (ESD)", fragment:"La descarga electrostática (ESD) puede dañar componentes electrónicos silenciosamente. Se previene usando pulseras antiestáticas y trabajando en superficies descargadas.", hint:"La ESD puede dañar sin notarse.", question:"¿Cómo se previene el daño por electricidad estática al manipular hardware?", options:["Usando guantes de goma","Usando pulsera antiestática conectada a tierra","Trabajando con el PC encendido","Usando ropa de algodón"], correct:1, explanation:"La pulsera antiestática (ESD strap) iguala el potencial eléctrico de la persona con el de la mesa de trabajo, evitando descargas que podrían dañar circuitos.", remember:"ESD: silenciosa pero destructiva. Siempre usar pulsera antiestática al manipular hardware.", goal:"Prevenir daños por electricidad estática" },
  { mission:"Velocidad del ventilador", fragment:"El RPM (revoluciones por minuto) de los ventiladores indica su velocidad. La mayoría de CPUs necesitan ventiladores de 1000-3000 RPM para operación normal.", hint:"RPM alto = más refrigeración y ruido.", question:"¿Qué controlador regula la velocidad de los ventiladores del PC según la temperatura?", options:["GPU","PWM (Pulse Width Modulation)","SATA","PCIe"], correct:1, explanation:"PWM (Modulación de Ancho de Pulso) permite al sistema ajustar la velocidad del ventilador según la temperatura: lento y silencioso en reposo, rápido bajo carga.", remember:"PWM controla velocidad del ventilador: temperatura ↑ → RPM ↑.", goal:"Entender el control PWM de ventiladores" },
  { mission:"Diagnóstico de PSU", fragment:"Una fuente de poder defectuosa puede causar apagones repentinos, inestabilidad o que el PC no encienda. El 'paper clip test' verifica si la PSU funciona independientemente.", hint:"La PSU puede fallar silenciosamente.", question:"¿Cuál es el síntoma más característico de una PSU defectuosa o insuficiente?", options:["Pantallas azules de RAM","Apagones aleatorios especialmente bajo carga","Lentitud al abrir programas","Ruido del disco duro"], correct:1, explanation:"Una PSU que no suministra suficiente energía causa apagones bajo carga alta (gaming, renders). Cambiar la PSU suele resolver estos fallos.", remember:"Apagones bajo carga + suficiente RAM y CPU = sospecha de PSU.", goal:"Diagnosticar fallos de la fuente de poder" },
  { mission:"Limpieza de conectores", fragment:"Los conectores oxidados o sucios en RAM, tarjetas PCIe y conectores SATA pueden causar errores de reconocimiento. La limpieza con borrador o alcohol isopropílico resuelve muchos fallos.", hint:"Alcohol isopropílico > alcohol normal.", question:"¿Qué sustancia es ideal para limpiar conectores eléctricos de hardware?", options:["Agua con jabón","Acetona","Alcohol isopropílico al 90%+","Vinagre blanco"], correct:2, explanation:"El alcohol isopropílico al 90% o más se evapora rápidamente sin dejar residuos conductores y limpia óxido y grasa sin dañar los circuitos.", remember:"Alcohol isopropílico ≥90% = limpiador ideal de electrónica.", goal:"Realizar limpieza correcta de conectores" },
  { mission:"Particionado de disco", fragment:"Particionar un disco divide su espacio en secciones lógicas independientes. MBR soporta hasta 4 particiones primarias y discos de hasta 2TB; GPT soporta 128 particiones y discos mayores.", hint:"GPT = moderno, sin límite de 2TB.", question:"¿Cuál tabla de particiones soporta discos de más de 2TB?", options:["MBR","FAT32","GPT","NTFS"], correct:2, explanation:"GPT (GUID Partition Table) soporta discos de hasta 9.4 ZB y 128 particiones. MBR (Master Boot Record) está limitado a 2TB y 4 particiones primarias.", remember:"GPT = moderno, UEFI, >2TB. MBR = legacy, BIOS, ≤2TB.", goal:"Elegir correctamente entre MBR y GPT" },
  { mission:"Monitoreo S.M.A.R.T.", fragment:"S.M.A.R.T. reporta atributos del disco como sectores reubicados, temperatura, horas de uso y errores. Un disco con muchos sectores defectuosos es candidato a fallo inminente.", hint:"Sectores reubicados = señal de alerta.", question:"¿Qué indica un alto número de 'Reallocated Sectors Count' en S.M.A.R.T.?", options:["Mayor velocidad del disco","El disco ha encontrado sectores defectuosos y los ha marcado","Mayor capacidad disponible","El disco está recién formateado"], correct:1, explanation:"Los sectores reubicados (Reallocated Sectors) indican que el disco encontró sectores dañados y los marcó como malos, usando sectores de reserva. Un número creciente indica fallo próximo.", remember:"Reallocated Sectors creciente = disco en deterioro. Hacer backup urgente.", goal:"Interpretar métricas S.M.A.R.T." },
  { mission:"Reinstalación del sistema operativo", fragment:"Reinstalar el SO resuelve problemas de software persistentes. La instalación limpia (fresh install) borra todo; la reparación conserva archivos y configuraciones.", hint:"Fresh install = borrón y cuenta nueva.", question:"¿Cuándo se recomienda una instalación limpia del SO en vez de reparación?", options:["Solo cuando el disco está lleno","Cuando hay infección de malware persistente o corrupción grave del sistema","Cuando el antivirus se desactualiza","Solo en nuevas instalaciones"], correct:1, explanation:"Una instalación limpia es la única garantía completa contra malware profundamente arraigado o corrupción grave del sistema. La reparación puede dejar rastros.", remember:"Malware persistente o SO muy dañado → instalación limpia.", goal:"Decidir entre reparación e instalación limpia" },
  { mission:"Solución de conflictos de drivers", fragment:"Los conflictos de drivers ocurren cuando dos controladores compiten por el mismo recurso o cuando un driver está desactualizado. El Administrador de dispositivos de Windows muestra el estado.", hint:"Administrador de dispositivos muestra conflictos.", question:"¿Qué indica un símbolo de exclamación amarillo en el Administrador de dispositivos?", options:["El dispositivo funciona perfectamente","El dispositivo tiene un problema o driver faltante/corrupto","El dispositivo está desconectado","El dispositivo necesita actualizarse"], correct:1, explanation:"El signo de exclamación en el Administrador de dispositivos indica que hay un problema con el driver o el hardware: puede estar corrupto, ausente o en conflicto.", remember:"⚠ en Adm. Dispositivos = problema de driver. Click derecho → Actualizar/Desinstalar driver.", goal:"Diagnosticar conflictos de drivers" },
  { mission:"Desfragmentación vs TRIM", fragment:"La desfragmentación reorganiza los archivos en HDDs para acceso continuo. En SSDs NO se debe desfragmentar, sino usar TRIM para notificar los bloques libres.", hint:"SSD + TRIM. HDD + desfragmentación.", question:"¿Por qué no se debe desfragmentar un SSD?", options:["Es muy lento","Reduce su vida útil al hacer escrituras innecesarias","No tiene efecto en velocidad","El SO no lo permite"], correct:1, explanation:"La desfragmentación en SSDs es contraproducente: cada celda NAND tiene un número limitado de ciclos de escritura. TRIM es suficiente para mantener el rendimiento del SSD.", remember:"HDD: desfragmentar. SSD: TRIM. No desfragmentar SSD.", goal:"Mantener correctamente HDDs y SSDs" },
  { mission:"Registro de Windows", fragment:"El Registro de Windows es una base de datos jerárquica que almacena configuraciones del sistema, drivers y aplicaciones. Modificarlo incorrectamente puede inutilizar el SO.", hint:"Regedit accede al registro.", question:"¿Cuál es el riesgo principal al editar manualmente el Registro de Windows?", options:["Reducir el espacio en disco","Dañar la configuración del SO y hacerlo no arrancar","Ralentizar la RAM","Desactivar el antivirus"], correct:1, explanation:"El registro contiene miles de entradas críticas. Un error al modificarlo puede impedir el inicio del sistema. Se recomienda hacer copia de seguridad antes de editar.", remember:"HKLM = configuración del sistema. HKCU = usuario. Siempre exportar antes de editar.", goal:"Comprender el Registro de Windows" },
  { mission:"Modo seguro de Windows", fragment:"El modo seguro inicia Windows con drivers mínimos, útil para diagnosticar problemas causados por drivers o malware. Se accede con F8 al arrancar o desde msconfig.", hint:"Drivers mínimos para diagnóstico.", question:"¿Para qué se usa principalmente el Modo Seguro en Windows?", options:["Para mejorar el rendimiento","Para diagnosticar y solucionar problemas de drivers o malware","Para instalar programas más rápido","Para conectarse a redes ocultas"], correct:1, explanation:"El Modo Seguro carga solo los drivers esenciales. Si el problema desaparece en Modo Seguro, el causante es un driver o programa de terceros cargado normalmente.", remember:"Modo Seguro: sin drivers de terceros = diagnóstico limpio.", goal:"Usar el Modo Seguro para diagnóstico" },
  { mission:"Energía no interrumpible", fragment:"Un UPS (Uninterruptible Power Supply) proporciona energía de reserva durante cortes eléctricos, protegiendo el hardware de apagones repentinos y picos de tensión.", hint:"UPS = batería de reserva.", question:"¿Cuál es la función principal de un UPS en un entorno de trabajo?", options:["Aumentar la velocidad del PC","Proveer energía temporal durante cortes y proteger contra picos","Mejorar la señal Wi-Fi","Refrigerar los servidores"], correct:1, explanation:"El UPS actúa como batería de emergencia y regulador de voltaje. Protege contra apagones que podrían corromper datos o dañar componentes sensibles.", remember:"UPS = protección contra apagones + regulación de voltaje.", goal:"Entender la importancia del UPS" },
  { mission:"Limpieza del sistema de archivos", fragment:"Los archivos temporales, cachés del sistema y puntos de restauración obsoletos ocupan espacio innecesario. La herramienta Liberador de espacio en disco (Disk Cleanup) los elimina.", hint:"Disk Cleanup y windir\\temp.", question:"¿Dónde se almacenan los archivos temporales de Windows?", options:["C:\\Windows\\System32","C:\\Users\\AppData\\Local\\Temp y C:\\Windows\\Temp","C:\\Program Files","C:\\Windows\\Fonts"], correct:1, explanation:"Windows guarda archivos temporales en %TEMP% (generalmente AppData\\Local\\Temp) y en Windows\\Temp. Limpiarlos periódicamente libera espacio en disco.", remember:"%TEMP% = carpeta de archivos temporales del usuario.", goal:"Limpiar archivos temporales del sistema" },
  { mission:"Puertos de diagnóstico", fragment:"Muchos equipos tienen puertos de diagnóstico como PS/2 (legacy), puerto COM serial y LEDs de diagnóstico en motherboards de gama alta que muestran códigos de error.", hint:"LEDs de diagnóstico en la motherboard.", question:"¿Qué función tienen los LEDs de diagnóstico en una motherboard de gama alta?", options:["Iluminar el interior del chasis","Mostrar qué componente falla durante el POST","Indicar la velocidad de la RAM","Controlar el ventilador de la CPU"], correct:1, explanation:"Las motherboards premium tienen LEDs o pantallitas de 7 segmentos que muestran códigos durante el POST para indicar qué componente está causando un fallo.", remember:"LED de diagnóstico: rojo en CPU/RAM/GPU/BOOT indica fallo en ese componente.", goal:"Usar LEDs de diagnóstico de la motherboard" },
  { mission:"Administración de energía", fragment:"La gestión de energía en Windows/Linux controla el rendimiento según el uso: Plan equilibrado, alto rendimiento y ahorro de energía afectan la velocidad del procesador.", hint:"Alto rendimiento = máxima velocidad.", question:"¿Qué plan de energía mantiene el CPU a máxima frecuencia constantemente?", options:["Ahorro de energía","Equilibrado","Alto rendimiento","Eco"], correct:2, explanation:"El plan de 'Alto rendimiento' desactiva el escalado dinámico de frecuencia, manteniendo la CPU al máximo. Consume más energía pero maximiza el rendimiento.", remember:"Alto rendimiento = máxima frecuencia. Equilibrado = automático. Ahorro = mínima frecuencia.", goal:"Configurar planes de energía" },
  { mission:"Reparación de MBR", fragment:"Un Master Boot Record (MBR) dañado impide arrancar el sistema. En Windows se puede reparar con bootrec /fixmbr y bootrec /fixboot desde la consola de recuperación.", hint:"bootrec es la herramienta de reparación.", question:"¿Qué comando repara el MBR en Windows desde la consola de recuperación?", options:["chkdsk /f","bootrec /fixmbr","diskpart /repair","msconfig /boot"], correct:1, explanation:"bootrec /fixmbr reescribe el código de arranque del MBR sin afectar la tabla de particiones. bootrec /fixboot repara el sector de arranque de la partición activa.", remember:"bootrec /fixmbr + bootrec /fixboot = reparación del arranque de Windows.", goal:"Reparar el sector de arranque de Windows" },
  { mission:"Ciclo de vida del hardware", fragment:"Los componentes de hardware tienen un ciclo de vida: diseño, fabricación, uso operativo y obsolescencia. Los HDDs tienen una vida media de 3-5 años; los SSDs, 5-10 años.", hint:"SSDs duran más que los HDDs mecánicos.", question:"¿Cuál es el principal indicador de obsolescencia de un componente de hardware?", options:["Color amarillento del plástico","No puede ejecutar el software actual o sus prestaciones son insuficientes","Mayor consumo de energía","Aumento del ruido del ventilador"], correct:1, explanation:"La obsolescencia funcional ocurre cuando el hardware no puede ejecutar el software actual (SO, aplicaciones) con rendimiento aceptable, independientemente de que siga funcionando.", remember:"Obsolescencia = no cumple requisitos actuales, no solo que esté roto.", goal:"Reconocer el ciclo de vida del hardware" },
  { mission:"Recuperación de datos", fragment:"Cuando un disco falla, herramientas como Recuva, TestDisk o R-Studio pueden recuperar datos de sectores aún legibles. La recuperación es más exitosa cuanto antes se intente.", hint:"No escribir al disco dañado aumenta las posibilidades.", question:"¿Qué se debe evitar para maximizar la recuperación de datos de un disco dañado?", options:["Apagar el computador","Continuar escribiendo datos en el disco dañado","Hacer una copia del disco","Usar TestDisk"], correct:1, explanation:"Escribir nuevos datos sobre un disco dañado puede sobrescribir los archivos que se intenta recuperar. Lo primero es crear una imagen del disco y trabajar sobre ella.", remember:"Disco dañado: primero clonar/imagen, luego recuperar. No escribir sobre él.", goal:"Aplicar procedimiento correcto de recuperación de datos" }
],

// WORLD 3 - Técnico
[
  { mission:"Subnetting básico", fragment:"El subnetting divide una red IP grande en subredes más pequeñas. Con una dirección /24 (255.255.255.0), tienes 256 IPs. Aumentar el prefijo divide la red: /25 = 2 subredes de 128.", hint:"Cada bit adicional divide la red en 2.", question:"¿Cuántas subredes crea un /26 a partir de una red /24?", options:["2","4","8","16"], correct:1, explanation:"Un prefijo /26 usa 2 bits más que /24 para la parte de red. 2^2 = 4 subredes, cada una con 64 IPs (62 usables). /24→/25→/26 duplica las subredes en cada paso.", remember:"Fórmula subredes: 2^(bits prestados). /26 toma 2 bits de /24 → 2^2=4 subredes.", goal:"Calcular subredes con CIDR" },
  { mission:"Protocolo BGP", fragment:"BGP (Border Gateway Protocol) es el protocolo de enrutamiento de Internet. Conecta Sistemas Autónomos (AS) entre ISPs. Es la columna vertebral del routing en Internet.", hint:"BGP conecta ISPs entre sí.", question:"¿En qué contexto se usa principalmente el protocolo BGP?", options:["Redes domésticas con un solo router","Enrutamiento entre diferentes Sistemas Autónomos en Internet","Redes LAN corporativas pequeñas","Control de VLANs en switches"], correct:1, explanation:"BGP (Border Gateway Protocol) es el único EGP (Exterior Gateway Protocol) en uso. Conecta los Sistemas Autónomos (AS) de diferentes organizaciones e ISPs para formar Internet.", remember:"BGP = EGP = Internet = entre AS. OSPF/EIGRP = IGP = dentro de un AS.", goal:"Comprender el protocolo BGP" },
  { mission:"Arquitectura RAID", fragment:"RAID 0 (striping) aumenta rendimiento sin redundancia. RAID 1 (mirroring) duplica datos para redundancia. RAID 5 (paridad distribuida) equilibra rendimiento y protección con mínimo 3 discos.", hint:"RAID 0: rápido pero sin protección.", question:"¿Cuántos discos necesita mínimamente el RAID 5?", options:["2","3","4","5"], correct:1, explanation:"RAID 5 requiere mínimo 3 discos y distribuye paridad entre todos. Puede perder 1 disco sin perder datos. Es el equilibrio ideal entre rendimiento, capacidad y redundancia.", remember:"RAID 0=rendimiento, RAID 1=redundancia, RAID 5=equilibrio (3 discos mín).", goal:"Seleccionar el nivel RAID adecuado" },
  { mission:"TLS/SSL", fragment:"TLS (Transport Layer Security) es el protocolo que cifra las comunicaciones en Internet (HTTPS). Reemplazó a SSL (ahora obsoleto) y usa criptografía asimétrica para el intercambio de claves.", hint:"HTTPS = HTTP + TLS.", question:"¿Cuál protocolo reemplazó a SSL como estándar de seguridad web?", options:["HTTP/2","TLS","IPsec","WPA3"], correct:1, explanation:"TLS 1.2 y 1.3 son las versiones actuales que reemplazaron a SSL. TLS 1.3 mejora velocidad (1-RTT handshake) y elimina algoritmos criptográficos débiles.", remember:"SSL = obsoleto. TLS 1.2/1.3 = estándar actual. TLS 1.3 = más rápido y seguro.", goal:"Comprender TLS y seguridad web" },
  { mission:"Protocolo OSPF avanzado", fragment:"OSPF utiliza áreas para escalar en redes grandes. El Área 0 (backbone) conecta todas las demás áreas. Los LSAs (Link State Advertisements) actualizan la topología.", hint:"Área 0 = backbone de OSPF.", question:"¿Cuál es la función del Área 0 en OSPF?", options:["Área para dispositivos IoT","Backbone que conecta todas las demás áreas","Área de alta seguridad","Área desactivada por defecto"], correct:1, explanation:"Toda red OSPF multi-área debe tener un Área 0 (backbone). Todas las demás áreas deben conectarse al backbone. Esto garantiza que el tráfico entre áreas pase por el backbone.", remember:"OSPF Área 0 = backbone obligatorio. Todas las áreas lo necesitan.", goal:"Diseñar redes OSPF multi-área" },
  { mission:"Virtualización", fragment:"La virtualización crea máquinas virtuales (VM) sobre hardware físico mediante un hipervisor. VMware ESXi y Hyper-V son hipervisores tipo 1 (bare-metal). VirtualBox es tipo 2.", hint:"Tipo 1 = sobre el hardware directamente.", question:"¿Cuál es la diferencia entre hipervisor Tipo 1 y Tipo 2?", options:["Tipo 1 es más lento","Tipo 1 corre directamente sobre el hardware; Tipo 2 sobre un SO","Tipo 2 es para servidores","Son idénticos en rendimiento"], correct:1, explanation:"Hipervisor Tipo 1 (bare-metal): VMware ESXi, Hyper-V, Xen. Corre directamente sobre el hardware sin SO host, mayor rendimiento. Tipo 2: VirtualBox, VMware Workstation, corre sobre SO.", remember:"Tipo 1 = bare-metal = más eficiente. Tipo 2 = sobre SO = más fácil.", goal:"Diferenciar tipos de hipervisores" },
  { mission:"Criptografía asimétrica", fragment:"La criptografía asimétrica usa par de claves: pública (cifra, se comparte) y privada (descifra, secreta). RSA, ECDSA y ED25519 son algoritmos comunes.", hint:"Clave pública cifra, privada descifra.", question:"En criptografía asimétrica, ¿qué clave se usa para CIFRAR el mensaje?", options:["La clave privada del emisor","La clave pública del receptor","La clave privada del receptor","Una clave simétrica compartida"], correct:1, explanation:"El emisor cifra con la clave pública del receptor. Solo el receptor puede descifrar con su clave privada. Esto garantiza confidencialidad sin necesidad de compartir secretos.", remember:"Cifrar: clave pública del receptor. Descifrar: clave privada del receptor.", goal:"Comprender la criptografía de clave pública" },
  { mission:"Protocolo SNMP", fragment:"SNMP (Simple Network Management Protocol) permite monitorear y gestionar dispositivos de red remotamente. Versión 3 añade autenticación y cifrado sobre las versiones 1 y 2c.", hint:"SNMPv3 = más seguro.", question:"¿Cuál versión de SNMP añade cifrado y autenticación?", options:["SNMPv1","SNMPv2","SNMPv2c","SNMPv3"], correct:3, explanation:"SNMPv3 incorpora mecanismos de seguridad: autenticación (MD5/SHA) y cifrado (DES/AES). Las versiones anteriores transmiten community strings en texto plano.", remember:"SNMPv1/v2c = texto plano, inseguro. SNMPv3 = autenticación + cifrado.", goal:"Seleccionar la versión correcta de SNMP" },
  { mission:"IPv6 avanzado", fragment:"IPv6 usa 128 bits en formato hexadecimal (8 grupos de 4 hex). La dirección ::1 es loopback. FE80::/10 son direcciones link-local. 2001::/32 es para Teredo.", hint:"::1 = loopback en IPv6.", question:"¿Cuál es la dirección de loopback en IPv6?", options:["127.0.0.1","::1","FE80::1","FF02::1"], correct:1, explanation:"En IPv6, ::1 (expandido: 0000:0000:0000:0000:0000:0000:0000:0001) es la dirección de loopback, equivalente a 127.0.0.1 en IPv4.", remember:"IPv4 loopback = 127.0.0.1. IPv6 loopback = ::1.", goal:"Trabajar con direcciones IPv6 especiales" },
  { mission:"Protocolo STP avanzado", fragment:"PVST+ (Per-VLAN Spanning Tree) crea una instancia de STP por VLAN, permitiendo balanceo de carga. MSTP agrupa VLANs en instancias para mayor eficiencia.", hint:"PVST+ = STP por VLAN.", question:"¿Qué ventaja tiene PVST+ sobre STP estándar (802.1D)?", options:["Más rápido en convergencia","Permite balanceo de carga por VLAN con instancias separadas","Elimina la necesidad del switch raíz","Compatible con todos los switches"], correct:1, explanation:"PVST+ permite que cada VLAN tenga su propio árbol de expansión y switch raíz, permitiendo usar diferentes puertos como activos en diferentes VLANs (balanceo de carga).", remember:"PVST+ = STP independiente por VLAN = posibilidad de balanceo de carga.", goal:"Optimizar redes con PVST+" },
  { mission:"ACL de red", fragment:"Las ACLs (Access Control Lists) en routers filtran tráfico basándose en IPs y puertos. ACL estándar (1-99): solo IP origen. ACL extendida (100-199): origen, destino, protocolo y puerto.", hint:"Estándar = solo IP origen.", question:"¿Qué tipo de ACL puede filtrar por dirección IP origen, destino, protocolo y puerto?", options:["ACL estándar (1-99)","ACL extendida (100-199)","ACL nombrada estándar","ACL de VLAN"], correct:1, explanation:"Las ACLs extendidas (100-199) son más granulares: pueden filtrar por IP origen, IP destino, protocolo (TCP/UDP/ICMP) y puertos específicos.", remember:"ACL estándar = solo origen. ACL extendida = origen+destino+protocolo+puerto.", goal:"Implementar ACLs correctamente en routers" },
  { mission:"Protocolo EIGRP", fragment:"EIGRP (Enhanced Interior Gateway Routing Protocol) es protocolo de enrutamiento híbrido de Cisco que combina características de vector de distancia y estado de enlace. Usa el algoritmo DUAL.", hint:"EIGRP usa el algoritmo DUAL.", question:"¿Qué algoritmo usa EIGRP para calcular rutas sin bucles?", options:["Dijkstra SPF","Bellman-Ford","DUAL (Diffusing Update Algorithm)","RIP vector"], correct:2, explanation:"EIGRP usa el algoritmo DUAL que garantiza rutas libres de bucles en todo momento. Mantiene rutas alternativas (Feasible Successor) para convergencia instantánea.", remember:"EIGRP: DUAL = rutas sin bucles + convergencia rápida. Cisco propietario (abierto desde 2013).", goal:"Comprender el protocolo EIGRP" },
  { mission:"Protocolo HTTPS y certificados", fragment:"Los certificados TLS son emitidos por CAs (Certificate Authorities). Contienen la clave pública del servidor, identidad y firma digital. El navegador verifica la firma de la CA.", hint:"CA firma el certificado del servidor.", question:"¿Qué información contiene un certificado TLS/SSL?", options:["Solo la clave privada del servidor","Clave pública, identidad del servidor y firma de la CA","Username y contraseña cifrados","Solo el nombre de dominio"], correct:1, explanation:"Un certificado X.509 contiene: clave pública, datos del propietario (CN, organización), datos de la CA emisora, periodo de validez y firma digital de la CA.", remember:"Certificado TLS = clave pública + identidad + firma CA. No contiene clave privada.", goal:"Comprender la infraestructura PKI" },
  { mission:"Modelo de seguridad Zero Trust", fragment:"Zero Trust asume que ningún usuario o dispositivo es confiable por defecto, ni siquiera dentro de la red corporativa. Verifica continuamente identidad, contexto y permisos.", hint:"Never trust, always verify.", question:"¿Cuál es el principio fundamental de la arquitectura Zero Trust?", options:["Confiar en los usuarios internos","Nunca confiar, siempre verificar","Usar solo firewalls perimetrales","Permitir todo el tráfico interno"], correct:1, explanation:"Zero Trust abandona el modelo de 'confiar en lo que está dentro del perímetro'. Toda solicitud se verifica independientemente del origen: usuario, dispositivo y contexto.", remember:"Zero Trust: Never trust, always verify. Seguridad basada en identidad, no en ubicación.", goal:"Comprender el modelo Zero Trust" },
  { mission:"DNS avanzado - Tipos de registros", fragment:"Los registros DNS incluyen A (IPv4), AAAA (IPv6), MX (correo), CNAME (alias), TXT (texto/SPF), NS (nameserver) y PTR (resolución inversa).", hint:"A = IPv4, AAAA = IPv6.", question:"¿Qué tipo de registro DNS asocia un nombre de dominio con una dirección IPv6?", options:["A","MX","AAAA","CNAME"], correct:2, explanation:"El registro AAAA (quad-A) asocia un nombre de dominio con una dirección IPv6 de 128 bits. El registro A hace lo mismo pero para IPv4 de 32 bits.", remember:"A = IPv4. AAAA = IPv6. MX = correo. CNAME = alias. TXT = texto.", goal:"Identificar los tipos de registros DNS" },
  { mission:"Tunneling y VPN", fragment:"VPN (Virtual Private Network) cifra el tráfico sobre redes públicas. Protocolos: OpenVPN, WireGuard, IPsec/IKEv2, L2TP. WireGuard destaca por simplicidad y velocidad.", hint:"WireGuard es el más moderno.", question:"¿Qué protocolo VPN es conocido por su código simple, alta velocidad y seguridad moderna?", options:["PPTP","L2TP/IPsec","OpenVPN","WireGuard"], correct:3, explanation:"WireGuard usa criptografía moderna (ChaCha20, Curve25519) con solo ~4000 líneas de código. Es más rápido que OpenVPN e IPsec, con menor superficie de ataque.", remember:"WireGuard = moderno, rápido, seguro, código mínimo. PPTP = obsoleto e inseguro.", goal:"Seleccionar protocolo VPN adecuado" },
  { mission:"SDN - Software Defined Networking", fragment:"SDN separa el plano de control (decisiones de enrutamiento) del plano de datos (reenvío de paquetes). Un controlador central gestiona toda la red programáticamente.", hint:"Plano control ≠ plano datos.", question:"¿Cuál es la característica principal de las redes SDN?", options:["Solo usan fibra óptica","Separación del plano de control y el plano de datos","Eliminan la necesidad de switches","Solo funcionan en la nube"], correct:1, explanation:"SDN centraliza el control en un software (controlador SDN como OpenDaylight). Los dispositivos de red solo reenvían paquetes según las reglas del controlador.", remember:"SDN: plano control (software centralizado) + plano datos (hardware). Programabilidad.", goal:"Comprender la arquitectura SDN" },
  { mission:"Protocolo Spanning Tree BPDU", fragment:"Los BPDU (Bridge Protocol Data Units) son los mensajes que intercambian los switches para ejecutar STP. Contienen información del switch raíz, costos de ruta y estados de puerto.", hint:"BPDU = mensajes de STP.", question:"¿Cuándo un switch envía BPDUs Topology Change Notification (TCN)?", options:["Al arrancar","Cuando un puerto cambia de estado (sube o baja)","Cada 30 segundos siempre","Solo cuando hay bucles"], correct:1, explanation:"Un switch envía TCN BPDU cuando detecta un cambio topológico (un puerto en estado forwarding pasa a down, por ejemplo), lo que hace que los otros switches vacíen sus tablas MAC.", remember:"TCN BPDU = aviso de cambio topológico. Genera limpieza de tablas MAC.", goal:"Entender el rol de los BPDUs en STP" },
  { mission:"Ataque Man-in-the-Middle", fragment:"Un ataque MITM intercepta comunicaciones entre dos partes. Técnicas: ARP spoofing, DNS poisoning, SSL stripping. TLS mutuo y DNSSEC mitigan estos ataques.", hint:"ARP spoofing = MITM en LAN.", question:"¿Qué técnica usa un atacante para realizar un MITM en una red LAN local?", options:["Inyección SQL","ARP Spoofing (envenenamiento de tablas ARP)","Fuerza bruta","Phishing"], correct:1, explanation:"ARP Spoofing envía respuestas ARP falsas para asociar la MAC del atacante con la IP de la víctima o el gateway, interceptando todo el tráfico.", remember:"ARP Spoofing = MITM en LAN. Defensa: ARP inspection dinámico en switches.", goal:"Identificar y defender contra ataques MITM" },
  { mission:"Protocolo MPLS", fragment:"MPLS (Multiprotocol Label Switching) usa etiquetas para enrutar paquetes más rápido que el enrutamiento IP tradicional. Es usado por ISPs para VPNs corporativas y QoS.", hint:"Etiquetas en vez de IPs para enrutar.", question:"¿Cuál es la principal ventaja de MPLS sobre el enrutamiento IP tradicional?", options:["Mayor seguridad por defecto","Enrutamiento más rápido usando etiquetas en vez de IPs","Menor costo de infraestructura","Compatible solo con IPv6"], correct:1, explanation:"MPLS usa etiquetas (labels) para tomar decisiones de reenvío en nanosegundos, sin analizar la cabecera IP completa. Esto mejora velocidad y permite QoS y VPNs de nivel carrier.", remember:"MPLS = etiquetas = reenvío rápido = QoS = VPNs carrier-grade.", goal:"Comprender MPLS en redes de proveedor" },
  { mission:"Certificación de red - Pruebas", fragment:"La certificación de instalaciones de red incluye pruebas de continuidad, longitud, atenuación, NEXT (diafonía) y retardo de propagación usando certificadoras como Fluke DSX.", hint:"NEXT = diafonía entre pares.", question:"¿Qué prueba mide la interferencia entre pares de cables en un cable UTP?", options:["Prueba de longitud","NEXT (Near-End Crosstalk)","Atenuación de retorno","Mapa de cables"], correct:1, explanation:"NEXT (Near-End Crosstalk) mide cuánta señal del par transmisor se acopla electromagnéticamente al par receptor. Un NEXT alto indica mala calidad del cable o conectores.", remember:"NEXT = diafonía en el extremo cercano. Crítico para Cat6/6a a 10Gbps.", goal:"Interpretar resultados de certificación de red" },
  { mission:"Protocolo 802.1X", fragment:"802.1X es el estándar de autenticación a nivel de puerto para redes LAN y Wi-Fi. Requiere que los dispositivos se autentiquen (via RADIUS) antes de acceder a la red.", hint:"RADIUS autentica en 802.1X.", question:"¿Qué protocolo de autenticación usa 802.1X para redes empresariales?", options:["DHCP","RADIUS/TACACS+","SNMP","NTP"], correct:1, explanation:"802.1X usa RADIUS (Remote Authentication Dial-In User Service) para autenticar dispositivos antes de darles acceso de red. El switch actúa como autenticador entre el cliente y el servidor RADIUS.", remember:"802.1X: suplicante (PC) → autenticador (switch) → servidor RADIUS.", goal:"Implementar autenticación 802.1X" },
  { mission:"Tabla de enrutamiento", fragment:"La tabla de enrutamiento de un router lista las redes conocidas y el next-hop para alcanzarlas. Tiene rutas directamente conectadas (C), estáticas (S) y dinámicas (O, R, D).", hint:"La tabla de enrutamiento es la base de las decisiones.", question:"¿Qué indica el código 'C' en una tabla de enrutamiento de un router Cisco?", options:["Ruta calculada por OSPF","Red directamente conectada","Ruta estática configurada","Ruta aprendida por BGP"], correct:1, explanation:"En IOS de Cisco, 'C' en la tabla de enrutamiento indica una red directamente conectada al router (una de sus interfaces). 'S' = estática, 'O' = OSPF, 'R' = RIP.", remember:"C = Conectada. S = Estática. O = OSPF. R = RIP. D = EIGRP. B = BGP.", goal:"Leer e interpretar tablas de enrutamiento" },
  { mission:"Protocolo NTP", fragment:"NTP (Network Time Protocol) sincroniza los relojes de los dispositivos de red. Es crítico para logs, certificados TLS, Kerberos y correlación de eventos de seguridad.", hint:"Sin NTP, los logs pierden coherencia.", question:"¿Por qué es crítica la sincronización de tiempo NTP en una red empresarial?", options:["Para mejorar la velocidad de la red","Para garantizar coherencia en logs, autenticación y certificados","Para asignar IPs automáticamente","Para balancear el tráfico"], correct:1, explanation:"NTP garantiza que todos los dispositivos tengan el mismo tiempo. Sin sincronización, los logs de diferentes sistemas no coinciden, los certificados TLS pueden fallar y Kerberos rechaza tokens.", remember:"NTP = tiempo sincronizado = logs coherentes = autenticación funcional.", goal:"Comprender la importancia de NTP" },
  { mission:"Protocolo LACP", fragment:"LACP (Link Aggregation Control Protocol - IEEE 802.3ad) combina múltiples enlaces físicos en un único enlace lógico, aumentando ancho de banda y proporcionando redundancia.", hint:"LACP = múltiples cables como uno.", question:"¿Cuál es el principal beneficio de implementar LACP entre switches?", options:["Reducir el número de VLANs","Combinar múltiples enlaces para mayor ancho de banda y redundancia","Aumentar la seguridad de la red","Simplificar la configuración de IP"], correct:1, explanation:"LACP (Link Aggregation Control Protocol) permite crear port-channels (EtherChannels) con hasta 8 enlaces activos, multiplicando el ancho de banda y eliminando ese enlace como punto único de fallo.", remember:"LACP/EtherChannel: hasta 8 enlaces = mayor BW + redundancia sin STP blocking.", goal:"Implementar agregación de enlaces con LACP" },
  { mission:"Direccionamiento VLSM", fragment:"VLSM (Variable Length Subnet Masking) permite usar máscaras de subred de diferentes longitudes en la misma red, optimizando el uso del espacio de direcciones IP.", hint:"Cada subred con el tamaño exacto que necesita.", question:"¿Cuál es la principal ventaja de VLSM sobre la división de subredes de longitud fija?", options:["Mayor velocidad de enrutamiento","Uso más eficiente del espacio de direcciones IP","Mayor compatibilidad con equipos legacy","Eliminación de la necesidad de NAT"], correct:1, explanation:"VLSM asigna a cada subred exactamente las IPs que necesita. Una red con 5 hosts usa /29 (8 IPs), otra con 100 hosts usa /25 (128 IPs), sin desperdiciar espacio.", remember:"VLSM = máscaras variables = optimización del espacio de direcciones.", goal:"Aplicar VLSM en diseño de redes" },
  { mission:"Protocolo HSRP/VRRP", fragment:"HSRP (Cisco) y VRRP (estándar) son protocolos de redundancia de gateway. Crean un IP virtual compartida entre dos routers; si el activo falla, el standby toma el control.", hint:"Gateway virtual = continuidad de servicio.", question:"¿Cuál es la diferencia entre HSRP y VRRP?", options:["HSRP es más rápido","HSRP es propietario de Cisco; VRRP es estándar IEEE","VRRP solo funciona con IPv6","Son exactamente iguales"], correct:1, explanation:"HSRP (Hot Standby Router Protocol) es propietario de Cisco. VRRP (Virtual Router Redundancy Protocol) es el estándar IEEE 2338 con funcionalidad equivalente. Ambos proveen redundancia de gateway.", remember:"HSRP = Cisco propietario. VRRP = estándar abierto. Mismo propósito: gateway redundante.", goal:"Implementar redundancia de gateway" }
]
];

// -------- INIT --------
function init() {
  loadState();
  renderWorldCards();
  updateHomeStats();
  document.getElementById('sound-toggle').onclick = toggleSound;
  document.addEventListener('keydown', handleKeydown);
  soundStart();
}

function loadState() {
  const saved = localStorage.getItem('studibotState');
  if (saved) {
    const s = JSON.parse(saved);
    state.score = s.score || 0;
    state.lives = s.lives || 3;
    state.ram = s.ram !== undefined ? s.ram : 30;
    state.record = s.record || 0;
    state.recordHistory = s.recordHistory || [];
    state.worldCompleted = s.worldCompleted || [false,false,false,false];
  }
}
function saveState() {
  localStorage.setItem('studibotState', JSON.stringify({
    score: state.score,
    lives: state.lives,
    ram: state.ram,
    record: state.record,
    recordHistory: state.recordHistory,
    worldCompleted: state.worldCompleted
  }));
}

function renderWorldCards() {
  const g = document.getElementById('worlds-grid');
  g.innerHTML = '';
  worlds.forEach((w, i) => {
    const card = document.createElement('div');
    card.className = 'world-card';
    card.style.borderColor = w.color + '44';
    const levelsDots = Array.from({length:30}, (_,j) => `<div class="level-dot ${state.worldCompleted[i] ? 'done' : ''}"></div>`).join('');
    card.innerHTML = `
      <span class="world-icon">${w.icon}</span>
      <div class="world-name">${w.name}</div>
      <div class="world-desc">${w.description}</div>
      <div class="world-levels">📊 30 NIVELES</div>
      <div class="level-dots" style="margin-top:10px;">${levelsDots}</div>
    `;
    card.onclick = () => openLesson(i);
    g.appendChild(card);
  });
}

function updateHomeStats() {
  document.getElementById('home-score').textContent = state.score;
  document.getElementById('home-lives').textContent = state.lives;
  document.getElementById('home-ram').textContent = state.ram;
  document.getElementById('home-record').textContent = state.record;
}

function openLesson(worldIdx) {
  soundSelect();
  state.currentWorld = worldIdx;
  const w = worlds[worldIdx];
  document.getElementById('lesson-icon').textContent = w.icon;
  document.getElementById('lesson-title').textContent = w.name;
  document.getElementById('lesson-subtitle').textContent = w.subtitle;
  document.getElementById('lesson-key-text').textContent = w.keyIdea;
  const cg = document.getElementById('lesson-concepts');
  cg.innerHTML = w.concepts.map(c => `
    <div class="concept-card">
      <span class="concept-icon">${c.icon}</span>
      <div class="concept-term">${c.term}</div>
      <div class="concept-def">${c.def}</div>
    </div>
  `).join('');
  showScreen('screen-lesson');
}

function startGame() {
  soundStart();
  state.currentLevelIndex = 0;
  buildLevelOrder();
  showScreen('screen-game');
  loadLevel();
}

function buildLevelOrder() {
  const bank = questionBanks[state.currentWorld];
  // Create a shuffled order of 30 levels (use modulo if bank < 30)
  const indices = [];
  for (let i = 0; i < 30; i++) indices.push(i % bank.length);
  // Shuffle
  for (let i = indices.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [indices[i], indices[j]] = [indices[j], indices[i]];
  }
  state.levelOrder = indices;
}

function loadLevel() {
  const bank = questionBanks[state.currentWorld];
  const lvlData = bank[state.levelOrder[state.currentLevelIndex]];
  state.hintRevealed = false;
  state.answered = false;

  // UI update
  document.getElementById('level-badge').textContent =
    `${worlds[state.currentWorld].icon} Nivel ${state.currentLevelIndex + 1}/30`;
  document.getElementById('goal-bar').innerHTML =
    `<span>META:</span> ${lvlData.goal}`;
  document.getElementById('mission-text').textContent = lvlData.mission;
  document.getElementById('fragment-text').textContent = lvlData.fragment;
  document.getElementById('hint-box').innerHTML = '💡 Pulsa el botón para revelar la pista';
  document.getElementById('hint-btn').disabled = false;
  document.getElementById('hint-btn').textContent = '🔍 PISTA (−25💾)';
  document.getElementById('question-text').textContent = lvlData.question;

  // Shuffle options
  const shuffled = shuffleOptions(lvlData.options, lvlData.correct);
  renderOptions(shuffled.options, shuffled.correctIdx, lvlData);

  // Progress bar
  const pct = (state.currentLevelIndex / 30) * 100;
  document.getElementById('prog-bar').style.width = pct + '%';

  // Level dots
  renderLevelDots();

  // Stats
  document.getElementById('game-score').textContent = state.score;
  document.getElementById('game-lives').textContent = state.lives;
  document.getElementById('game-ram').textContent = state.ram;

  // Timer
  startTimer();
}

function shuffleOptions(options, correctIdx) {
  const indexed = options.map((o, i) => ({text: o, orig: i}));
  for (let i = indexed.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [indexed[i], indexed[j]] = [indexed[j], indexed[i]];
  }
  const newCorrectIdx = indexed.findIndex(o => o.orig === correctIdx);
  return { options: indexed.map(o => o.text), correctIdx: newCorrectIdx };
}

function renderOptions(options, correctIdx, lvlData) {
  const letters = ['A', 'B', 'C', 'D'];
  const container = document.getElementById('options-container');
  container.innerHTML = '';
  options.forEach((opt, i) => {
    const btn = document.createElement('button');
    btn.className = 'option-btn';
    btn.innerHTML = `<span class="opt-letter">${letters[i]}</span><span>${opt}</span>`;
    btn.onclick = () => selectOption(btn, i, correctIdx, lvlData);
    container.appendChild(btn);
  });
}

function renderLevelDots() {
  const d = document.getElementById('level-dots');
  d.innerHTML = '';
  for (let i = 0; i < 30; i++) {
    const dot = document.createElement('div');
    dot.className = 'level-dot' + (i < state.currentLevelIndex ? ' done' : i === state.currentLevelIndex ? ' current' : '');
    d.appendChild(dot);
  }
}

// Timer
function startTimer() {
  clearInterval(state.timerInterval);
  state.timerSeconds = 30;
  updateTimerDisplay();
  state.timerInterval = setInterval(() => {
    if (state.paused || state.answered) return;
    state.timerSeconds--;
    updateTimerDisplay();
    if (state.timerSeconds <= 5) { soundAlert(); }
    if (state.timerSeconds <= 0) {
      clearInterval(state.timerInterval);
      timeUp();
    }
  }, 1000);
}

function updateTimerDisplay() {
  const t = document.getElementById('timer-text');
  const bar = document.getElementById('timer-bar');
  t.textContent = state.timerSeconds;
  const circumference = 125.66;
  const pct = state.timerSeconds / 30;
  bar.style.strokeDashoffset = circumference * (1 - pct);
  if (state.timerSeconds <= 5) {
    bar.style.stroke = '#f44336';
    t.style.color = '#f44336';
  } else if (state.timerSeconds <= 10) {
    bar.style.stroke = '#ff9800';
    t.style.color = '#ff9800';
  } else {
    bar.style.stroke = 'var(--cyan)';
    t.style.color = 'var(--white)';
  }
}

function timeUp() {
  state.lives--;
  document.getElementById('game-lives').textContent = state.lives;
  // Disable all options
  document.querySelectorAll('.option-btn').forEach(b => b.disabled = true);
  if (state.lives <= 0) {
    setTimeout(gameOver, 800);
  } else {
    showToast('⏰ ¡Tiempo agotado! -1 ❤️');
    // show correct answer
    const bank = questionBanks[state.currentWorld];
    const lvlData = bank[state.levelOrder[state.currentLevelIndex]];
    showFeedback(false, lvlData, 0);
  }
  saveState();
}

function selectOption(btn, idx, correctIdx, lvlData) {
  if (state.answered) return;
  state.answered = true;
  clearInterval(state.timerInterval);
  soundSelect();

  const allBtns = document.querySelectorAll('.option-btn');
  allBtns.forEach(b => b.disabled = true);

  const isCorrect = idx === correctIdx;

  if (isCorrect) {
    btn.classList.add('correct');
    soundCorrect();
    launchConfetti();
    // Score: proportional to time remaining
    const points = Math.max(2, Math.round(2 + (state.timerSeconds / 30) * 8));
    state.score += points;
    state.ram += 1;
    showFeedback(true, lvlData, points);
  } else {
    btn.classList.add('wrong');
    allBtns[correctIdx].classList.add('correct');
    soundWrong();
    state.lives--;
    document.getElementById('game-lives').textContent = state.lives;
    if (state.lives <= 0) {
      setTimeout(gameOver, 1200);
      return;
    }
    showFeedback(false, lvlData, 0);
  }

  if (state.score > state.record) {
    state.record = state.score;
    state.recordHistory.push({ score: state.score, world: worlds[state.currentWorld].name, date: new Date().toLocaleDateString() });
    if (state.recordHistory.length > 10) state.recordHistory.shift();
  }
  document.getElementById('game-score').textContent = state.score;
  document.getElementById('game-ram').textContent = state.ram;
  saveState();
}

function showFeedback(correct, lvlData, points) {
  document.getElementById('fb-icon').textContent = correct ? '✅' : '❌';
  document.getElementById('fb-title').textContent = correct ? '¡Respuesta Correcta!' : 'Respuesta Incorrecta';
  document.getElementById('fb-title').className = 'modal-result-title ' + (correct ? 'ok' : 'fail');
  document.getElementById('fb-score').textContent = correct ? `+${points} pts | +1💾 RAM` : '−1 ❤️';
  document.getElementById('fb-explanation').textContent = lvlData.explanation;
  document.getElementById('fb-remember').textContent = '💡 ' + lvlData.remember;
  document.getElementById('modal-feedback').classList.add('active');
}

function nextLevel() {
  closeModal('modal-feedback');
  state.currentLevelIndex++;
  if (state.currentLevelIndex >= 30) {
    worldComplete();
  } else {
    loadLevel();
  }
}

function worldComplete() {
  clearInterval(state.timerInterval);
  state.worldCompleted[state.currentWorld] = true;
  launchConfetti();
  const ws = document.getElementById('win-stats');
  ws.innerHTML = `
    <div class="go-stat"><span class="gsl">Mundo</span><span class="gsv">${worlds[state.currentWorld].name}</span></div>
    <div class="go-stat"><span class="gsl">Puntaje final</span><span class="gsv">${state.score}</span></div>
    <div class="go-stat"><span class="gsl">RAMs obtenidas</span><span class="gsv">${state.ram} 💾</span></div>
    <div class="go-stat"><span class="gsl">Récord</span><span class="gsv">${state.record}</span></div>
  `;
  saveState();
  renderWorldCards();
  document.getElementById('modal-win').classList.add('active');
}

function gameOver() {
  clearInterval(state.timerInterval);
  const gs = document.getElementById('go-stats');
  gs.innerHTML = `
    <div class="go-stat"><span class="gsl">Puntaje alcanzado</span><span class="gsv">${state.score}</span></div>
    <div class="go-stat"><span class="gsl">Nivel llegado</span><span class="gsv">${state.currentLevelIndex + 1}/30</span></div>
    <div class="go-stat"><span class="gsl">Récord histórico</span><span class="gsv">${state.record}</span></div>
    <div class="go-stat"><span class="gsl">RAMs restantes</span><span class="gsv">${state.ram} 💾</span></div>
  `;
  document.getElementById('modal-gameover').classList.add('active');
}

function restartFromGameOver() {
  closeModal('modal-gameover');
  state.lives = 3;
  state.score = 0;
  state.ram = 30;
  saveState();
  updateHomeStats();
  buildLevelOrder();
  state.currentLevelIndex = 0;
  showScreen('screen-game');
  loadLevel();
}

function revealHint() {
  if (state.hintRevealed) return;
  if (state.ram < 25) {
    showToast('❌ No tienes suficientes RAMs (necesitas 25💾)');
    return;
  }
  state.ram -= 25;
  state.hintRevealed = true;
  const bank = questionBanks[state.currentWorld];
  const lvlData = bank[state.levelOrder[state.currentLevelIndex]];
  document.getElementById('hint-box').innerHTML = `💡 ${lvlData.hint}`;
  document.getElementById('hint-btn').disabled = true;
  document.getElementById('hint-btn').textContent = '✓ Pista revelada';
  document.getElementById('game-ram').textContent = state.ram;
  saveState();
}

// ---- PAUSE ----
function openPause() {
  state.paused = true;
  document.getElementById('modal-pause').classList.add('active');
}
function closePause() {
  state.paused = false;
  closeModal('modal-pause');
}

// ---- RESET ----
function resetCurrentLevel() {
  closeModal('modal-pause');
  state.paused = false;
  buildLevelOrder();
  state.currentLevelIndex = Math.max(0, state.currentLevelIndex);
  loadLevel();
}
function resetAll() {
  state.score = 0;
  state.lives = 3;
  state.ram = 30;
  state.worldCompleted = [false,false,false,false];
  saveState();
  updateHomeStats();
  renderWorldCards();
  closeModal('modal-gameover');
  closeModal('modal-pause');
  showScreen('screen-home');
}

// ---- RECORD ----
function showRecord() {
  document.getElementById('record-display').textContent = state.record;
  const hist = document.getElementById('record-history');
  if (state.recordHistory.length === 0) {
    hist.innerHTML = '<div style="color:var(--text-dim);font-size:0.8rem;text-align:center;">Aún no hay intentos registrados.</div>';
  } else {
    hist.innerHTML = state.recordHistory.slice().reverse().map((r, i) => `
      <div class="go-stat">
        <span class="gsl">#${i+1} ${r.world} — ${r.date}</span>
        <span class="gsv">${r.score} pts</span>
      </div>
    `).join('');
  }
  document.getElementById('modal-record').classList.add('active');
}

// ---- SOUND ----
function toggleSound() {
  state.soundOn = !state.soundOn;
  document.getElementById('sound-toggle').textContent = state.soundOn ? '🔊' : '🔇';
}

// ---- NAVIGATION ----
function showScreen(id) {
  clearInterval(state.timerInterval);
  closeModal('modal-feedback');
  closeModal('modal-pause');
  closeModal('modal-gameover');
  closeModal('modal-win');
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if (id === 'screen-home') { updateHomeStats(); renderWorldCards(); }
}

function closeModal(id) {
  document.getElementById(id).classList.remove('active');
}

function handleKeydown(e) {
  if (e.key === 'p' || e.key === 'P') {
    const gameActive = document.getElementById('screen-game').classList.contains('active');
    if (gameActive) {
      if (state.paused) closePause(); else openPause();
    }
  }
}

// ---- START ----
window.addEventListener('load', init);
</script>
</body>
</html>
