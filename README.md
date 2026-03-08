<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Go Up!</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@700;900&family=Share+Tech+Mono&display=swap');
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{--ui:'Orbitron',monospace;--mono:'Share Tech Mono',monospace}
html,body{width:100%;height:100%;margin:0;padding:0;background:#07050f;overflow:hidden;font-family:var(--ui);color:#fff;touch-action:none;user-select:none;-webkit-user-select:none}
#app{display:flex;flex-direction:column;align-items:stretch;width:100%;height:100%;position:fixed;inset:0}
#gw{display:flex;flex-direction:column;width:100%;height:100%;}

.screen{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:100;overflow:hidden}
.screen.hidden{display:none}
.sbg{position:absolute;inset:0;background:radial-gradient(ellipse at 50% 55%,#1a0a3a 0%,#07050f 70%)}

.gtitle{position:relative;z-index:2;font-size:clamp(3.5rem,13vw,7rem);font-weight:900;letter-spacing:.06em;text-transform:uppercase;
  background:linear-gradient(135deg,#7b2fff 0%,#00e5ff 50%,#ff6aff 100%);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
  filter:drop-shadow(0 0 30px #7b2fff99);margin-bottom:.1em;animation:pt 2.5s ease-in-out infinite}
@keyframes pt{0%,100%{filter:drop-shadow(0 0 24px #7b2fff88)}50%{filter:drop-shadow(0 0 48px #00e5ffaa)}}
.gsub{position:relative;z-index:2;font-family:var(--mono);font-size:clamp(.7rem,2vw,.9rem);color:#555;margin-bottom:2.2em;letter-spacing:.22em}
.ghint{position:relative;z-index:2;font-family:var(--mono);font-size:clamp(.6rem,1.6vw,.78rem);color:#2e2e2e;margin-bottom:2em;text-align:center;line-height:2.2}
.gval{position:relative;z-index:2;font-family:var(--mono);font-size:clamp(.9rem,2.6vw,1.3rem);color:#ccc;margin-bottom:.4em}
.gval span{color:#00e5ff;text-shadow:0 0 10px #00e5ff55}

/* ── base btn — uniform size, colour-matched fill on hover ── */
.btn{position:relative;z-index:2;font-family:var(--ui);
  font-size:clamp(.78rem,2.2vw,.95rem);font-weight:700;letter-spacing:.13em;
  text-transform:uppercase;padding:.78em 0;border-radius:5px;background:transparent;
  cursor:pointer;transition:all .18s;margin:.35em;overflow:hidden;
  width:clamp(200px,62vw,280px);text-align:center;
  /* default purple */
  border:2px solid #7b2fff;color:#7b2fff}
.btn::before{content:'';position:absolute;inset:0;
  background:linear-gradient(135deg,var(--btn-c1,#7b2fff),var(--btn-c2,#7b2fff));
  transform:scaleX(0);transform-origin:left;transition:transform .2s;z-index:-1}
.btn:hover::before,.btn:active::before{transform:scaleX(1)}
.btn:hover,.btn:active{color:#000;box-shadow:0 0 22px var(--btn-glow,#7b2fff88)}

/* cyan buttons */
#btnWard,#btnWardSave{--btn-c1:#00e5ff;--btn-c2:#00b8cc;--btn-glow:#00e5ff88;border-color:#00e5ff;color:#00e5ff}
/* pink buttons */
#btnStyle,#btnStyleBack{--btn-c1:#ff6aff;--btn-c2:#cc00cc;--btn-glow:#ff6aff88;border-color:#ff6aff;color:#ff6aff}
/* restart */
#btnRestart{--btn-c1:#7b2fff;--btn-c2:#00e5ff;--btn-glow:#7b2fff88}

#gw{display:flex;flex-direction:column;width:100%;height:100%}
#hud{width:100%;display:flex;justify-content:space-between;align-items:center;
  padding:6px 14px;background:rgba(0,0,0,.65);border-bottom:1px solid #ffffff10;flex-shrink:0;z-index:10}
.hi{font-family:var(--mono);font-size:clamp(.62rem,2vw,.86rem);color:#888;display:flex;flex-direction:column;align-items:center;gap:1px}
.hi .lb{font-size:.6em;color:#333;letter-spacing:.1em;text-transform:uppercase}
.hi .vl{color:#fff;font-weight:bold}
#cc{flex:1;width:100%;position:relative;overflow:hidden;background:#07050f}
#gc{display:block;width:100%;height:100%}

#tc{display:none;width:100%;padding:10px 18px 14px;background:rgba(0,0,0,.82);border-top:1px solid #ffffff0c;flex-shrink:0}
.tr{display:flex;justify-content:space-between;align-items:center}
.ts{display:flex;gap:10px}
.vb{width:clamp(62px,14vw,80px);height:clamp(54px,12vw,68px);border-radius:10px;border:2px solid #ffffff12;
  background:rgba(255,255,255,.04);display:flex;align-items:center;justify-content:center;
  cursor:pointer;-webkit-tap-highlight-color:transparent;touch-action:manipulation;color:#444;font-size:clamp(1.2rem,5vw,1.8rem)}
.vb.active{background:rgba(123,47,255,.22);border-color:#7b2fff;color:#00e5ff}
.jb{width:clamp(72px,16vw,94px);background:rgba(0,229,255,.07);border-color:#00e5ff;color:#00e5ff;
  font-size:clamp(.66rem,2.3vw,.86rem);font-family:var(--ui);font-weight:700}
@media(hover:none)and(pointer:coarse){#tc{display:flex;flex-direction:column}}

#cf{position:absolute;top:38%;left:50%;transform:translate(-50%,-50%);font-family:var(--ui);
  font-size:clamp(1rem,3.5vw,1.3rem);font-weight:700;color:#00ff88;text-shadow:0 0 22px #00ff88;
  opacity:0;pointer-events:none;z-index:20;transition:opacity .22s;letter-spacing:.12em;text-transform:uppercase;white-space:nowrap}
#cf.show{opacity:1}
/* wardrobe */
#scrWard{background:#07050f}
.ward-inner{position:relative;z-index:2;display:flex;flex-direction:column;align-items:center;gap:1.1em}
.ward-title{font-size:clamp(1.4rem,5vw,2.2rem);font-weight:900;letter-spacing:.1em;
  background:linear-gradient(135deg,#7b2fff,#00e5ff);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
#colWheel{border-radius:50%;cursor:crosshair;touch-action:none;image-rendering:crisp-edges}
.ward-preview-wrap{display:flex;flex-direction:column;align-items:center;gap:.5em}
.ward-preview-label{font-family:var(--mono);font-size:.7rem;color:#444;letter-spacing:.12em}
#wardPreview{width:52px;height:52px;border-radius:7px;border:2px solid #ffffff18;box-shadow:0 0 18px var(--pc,#7b2fff)}
.ward-hex{font-family:var(--mono);font-size:.78rem;color:#666;letter-spacing:.1em}
/* difficulty */
#scrDiff{background:#07050f}
.diff-wrap{position:relative;z-index:2;display:flex;flex-direction:column;align-items:center;gap:1.2em;padding:1em}
.diff-title{font-size:clamp(1.2rem,4vw,1.9rem);font-weight:900;letter-spacing:.12em;color:#aaa;margin-bottom:.2em}
/* difficulty buttons — all same size */
.dbtn{position:relative;overflow:hidden;cursor:pointer;border:none;outline:none;
  font-family:var(--ui);font-weight:700;letter-spacing:.13em;text-transform:uppercase;
  padding:.82em 0;border-radius:6px;width:clamp(220px,68vw,300px);text-align:center;
  transition:transform .12s,box-shadow .15s,border-color .15s;font-size:clamp(.82rem,2.3vw,1rem);
  border:2px solid transparent}
.dbtn:active{transform:scale(.97)}

/* EASY — dim neon green, low energy */
#dEasy{background:#0d160d;color:#4a7a4a;border-color:#2a4a2a;
  box-shadow:0 0 8px #2a5a2a33;letter-spacing:.08em}
#dEasy:hover{background:#111e11;color:#6aaa6a;border-color:#3a6a3a;box-shadow:0 0 14px #3a6a3a55}
#dEasy::before{content:'';position:absolute;inset:0;
  background:linear-gradient(135deg,rgba(40,80,40,.2),transparent);pointer-events:none}

/* MEDIUM — purple/blue neon glow */
#dMed{background:linear-gradient(135deg,#120830,#080e24);color:#99bbff;border-color:#4466cc;
  box-shadow:0 0 16px #4466cc55,0 0 32px #4466cc22}
#dMed:hover{box-shadow:0 0 28px #5588ff88,0 0 55px #4466cc44;border-color:#6699ff;color:#cce0ff}
#dMed::after{content:'';position:absolute;inset:0;
  background:linear-gradient(135deg,rgba(80,120,255,.12),rgba(0,180,255,.07));pointer-events:none}

/* HARDCORE — electric blue neon */
#dHard{background:#00050f;color:#00ccff;border-color:#00aaff;
  box-shadow:0 0 22px #00aaff99,0 0 55px #0066ff44,inset 0 0 18px #00224411;
  animation:hardPulse 1.1s ease-in-out infinite}
#dHard:hover{box-shadow:0 0 40px #00ccffcc,0 0 90px #0088ff77;color:#fff;border-color:#00ddff}
@keyframes hardPulse{
  0%,100%{box-shadow:0 0 20px #00aaff88,0 0 45px #0066ff33}
  50%{box-shadow:0 0 36px #00ccffcc,0 0 80px #0088ff66,inset 0 0 28px #003366}
}
#dHard::before{content:'';position:absolute;inset:0;pointer-events:none;
  background:linear-gradient(135deg,rgba(0,150,255,.14),rgba(0,220,255,.06),rgba(0,100,220,.1))}
.diff-back{font-family:var(--mono);font-size:.72rem;color:#333;cursor:pointer;
  letter-spacing:.1em;text-decoration:none;margin-top:.4em}
.diff-back:hover{color:#777}
/* style screen */
.style-inner{position:relative;z-index:2;display:flex;flex-direction:column;align-items:center;gap:.9em;padding:1em;width:100%;max-width:480px}
.style-title{font-size:clamp(1.4rem,5vw,2rem);font-weight:900;letter-spacing:.1em;
  background:linear-gradient(135deg,#ff6aff,#00e5ff,#ffcc00);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.style-sub{font-family:var(--mono);font-size:.7rem;color:#444;letter-spacing:.1em;text-align:center}
.style-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:.7em;width:100%;max-width:400px}
.scard{cursor:pointer;border-radius:10px;padding:.7em .9em;border:2px solid transparent;
  display:flex;align-items:center;gap:.7em;transition:all .18s;position:relative;overflow:hidden}
.scard:hover{transform:scale(1.03);filter:brightness(1.15)}
.scard.active{border-color:#fff!important;box-shadow:0 0 18px rgba(255,255,255,.3)!important}
.scard-cube{width:28px;height:28px;border-radius:5px;flex-shrink:0}
.scard-info{display:flex;flex-direction:column;gap:2px}
.scard-name{font-family:var(--ui);font-size:.72rem;font-weight:700;letter-spacing:.08em;color:#fff}
.scard-desc{font-family:var(--mono);font-size:.58rem;color:rgba(255,255,255,.5);letter-spacing:.05em}
#voidTimer{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);
  font-family:var(--ui);font-size:clamp(1.2rem,5vw,2rem);font-weight:900;color:#ff4444;
  text-shadow:0 0 20px #ff4444;opacity:0;pointer-events:none;z-index:25;transition:opacity .15s;
  letter-spacing:.1em;white-space:nowrap}
#voidTimer.show{opacity:1}

/* unlock popup */
#unlockPop{position:absolute;top:14px;left:50%;transform:translateX(-50%);
  font-family:var(--ui);font-size:clamp(.75rem,2.8vw,1rem);font-weight:700;
  letter-spacing:.1em;text-transform:uppercase;white-space:nowrap;
  padding:.5em 1.2em;border-radius:8px;border:2px solid;
  pointer-events:none;z-index:28;opacity:0;transition:opacity .3s;
  text-align:center;line-height:1.4}
#unlockPop.show{opacity:1}
#unlockPop.triple{color:#ff6aff;border-color:#ff6aff;background:rgba(30,0,40,.85);
  box-shadow:0 0 24px #ff6aff99,0 0 60px #ff6aff44}
#unlockPop.dash{color:#00e5ff;border-color:#00e5ff;background:rgba(0,20,35,.88);
  box-shadow:0 0 24px #00e5ff99,0 0 60px #00e5ff44}
.dash-cd{font-size:.55em;font-family:var(--mono);color:#aaa;margin-top:2px}

/* redeem button — top right, plain */
#btnRedeem{position:absolute;top:14px;right:14px;z-index:10;
  font-family:var(--mono);font-size:.68rem;letter-spacing:.08em;color:#444;
  background:rgba(255,255,255,.04);border:1px solid #222;border-radius:5px;
  padding:.4em .8em;cursor:pointer;transition:color .15s,border-color .15s}
#btnRedeem:hover{color:#aaa;border-color:#555}
/* modals shared */
#redeemBox,#tpBox{background:#0d0b18;border:1px solid #2a2040;border-radius:12px;
  padding:1.6em 2em;display:flex;flex-direction:column;gap:.7em;align-items:center;
  min-width:clamp(260px,70vw,360px);box-shadow:0 0 40px rgba(0,0,0,.8)}
#redeemTitle,#tpTitle{font-family:var(--ui);font-size:1rem;font-weight:700;letter-spacing:.1em;color:#ccc}
#redeemInput,#tpInput{font-family:var(--mono);font-size:.9rem;letter-spacing:.1em;
  background:#080612;border:1px solid #2a2040;border-radius:6px;color:#fff;
  padding:.55em 1em;width:100%;outline:none;text-align:center}
#redeemInput:focus,#tpInput:focus{border-color:#5533aa}
#redeemMsg,#tpMsg{font-family:var(--mono);font-size:.72rem;letter-spacing:.05em;min-height:1.1em;text-align:center}
#redeemOk,#tpOk{font-family:var(--ui);font-size:.75rem;font-weight:700;letter-spacing:.1em;
  padding:.5em 1.4em;border-radius:5px;cursor:pointer;background:transparent;
  border:1px solid #5533aa;color:#aa88ff;transition:all .15s}
#redeemOk:hover,#tpOk:hover{background:#5533aa22;color:#cbb0ff}
#redeemClose,#tpClose{font-family:var(--mono);font-size:.72rem;letter-spacing:.08em;
  padding:.5em 1em;border-radius:5px;cursor:pointer;background:transparent;
  border:1px solid #222;color:#444;transition:all .15s}
#redeemClose:hover,#tpClose:hover{border-color:#444;color:#888}
.msg-ok{color:#00ff88}.msg-err{color:#ff4444}.msg-already{color:#ffcc00}

#scrWard .btn{margin-top:.4em}
#po{position:absolute;inset:0;background:rgba(0,0,0,.55);display:flex;align-items:center;justify-content:center;
  font-size:2rem;font-weight:900;letter-spacing:.22em;color:#fff;z-index:30;pointer-events:none;opacity:0;transition:opacity .2s}
#po.on{opacity:1}
</style>
</head>
<body>
<div id="app">

<!-- START -->
<div class="screen" id="scrStart">
  <div class="sbg"></div>
  <button id="btnRedeem">🎟 Redeem Code</button>
  <div class="gtitle">Go Up!</div>
  <div class="gsub">climb as high as you can</div>
  <div class="ghint">WASD / ARROWS — move &amp; jump<br>Jump again in mid-air for double jump<br>Green platforms = checkpoints</div>
  <button class="btn" id="btnPlay">▶ PLAY</button>
  <button class="btn" id="btnWard">🎨 WARDROBE</button>
  <button class="btn" id="btnStyle">✦ STYLE</button>
</div>

<!-- DIFFICULTY -->
<div class="screen hidden" id="scrDiff">
  <div class="sbg"></div>
  <div class="diff-wrap">
    <div class="diff-title">Choose Difficulty</div>
    <button class="dbtn" id="dEasy">easy</button>
    <button class="dbtn" id="dMed">✦ Medium ✦</button>
    <button class="dbtn" id="dHard">💀 HARDCORE 💀</button>
    <span class="diff-back" id="diffBack">← back</span>
  </div>
</div>

<!-- WARDROBE -->
<div class="screen hidden" id="scrWard">
  <div class="sbg"></div>
  <div class="ward-inner">
    <div class="ward-title">🎨 Wardrobe</div>
    <canvas id="colWheel" width="220" height="210"></canvas>
    <div class="ward-preview-wrap">
      <div class="ward-preview-label">YOUR CUBE</div>
      <div id="wardPreview"></div>
      <div class="ward-hex" id="wardHex">#7b2fff</div>
    </div>
    <button class="btn" id="btnWardSave">✔ SAVE &amp; BACK</button>
  </div>
</div>

<!-- GAME OVER -->
<div class="screen hidden" id="scrOver">
  <div class="sbg"></div>
  <div class="gtitle" style="font-size:clamp(1.8rem,8vw,3.5rem)">Game Over</div>
  <div class="gval" style="margin-top:.8em">Platforms visited: <span id="txScore">0</span></div>
  <div class="gval">🏆 High Score: <span id="txHigh" style="color:#ffd700">0</span></div>
  <button class="btn" id="btnRestart">↺ PLAY AGAIN</button>
</div>

<!-- STYLE -->
<div class="screen hidden" id="scrStyle">
  <div class="sbg"></div>
  <div class="style-inner">
    <div class="style-title">✦ Style</div>
    <div class="style-sub">pick a vibe — changes background &amp; cube colour</div>
    <div class="style-grid" id="styleGrid"></div>
    <button class="btn" id="btnStyleBack" style="margin-top:.8em;font-size:.8rem">← BACK</button>
  </div>
</div>

<!-- REDEEM MODAL -->
<div id="redeemModal" style="display:none;position:fixed;inset:0;z-index:200;background:rgba(0,0,0,.7);
  display:none;align-items:center;justify-content:center">
  <div id="redeemBox">
    <div id="redeemTitle">🎟 Redeem Code</div>
    <input id="redeemInput" type="text" placeholder="Enter code…" autocomplete="off" spellcheck="false"/>
    <div id="redeemMsg"></div>
    <div style="display:flex;gap:.6em;justify-content:center;margin-top:.4em">
      <button id="redeemOk">REDEEM</button>
      <button id="redeemClose">CANCEL</button>
    </div>
  </div>
</div>

<!-- TELEPORT MODAL -->
<div id="tpModal" style="display:none;position:fixed;inset:0;z-index:200;background:rgba(0,0,0,.75);
  align-items:center;justify-content:center">
  <div id="tpBox">
    <div id="tpTitle">⬆ Teleport to Platform</div>
    <input id="tpInput" type="number" min="1" placeholder="Platform number…" autocomplete="off"/>
    <div id="tpMsg"></div>
    <div style="display:flex;gap:.6em;justify-content:center;margin-top:.4em">
      <button id="tpOk">GO</button>
      <button id="tpClose">CANCEL</button>
    </div>
  </div>
</div>

<!-- GAME -->
<div id="gw" style="display:none">
  <div id="hud">
    <div class="hi"><div class="lb">Platforms</div><div class="vl" id="hH">0</div></div>
    <div class="hi"><div class="lb">Best</div><div class="vl" id="hB">0m</div></div>
    <div class="hi"><div class="lb">🏆 High</div><div class="vl" id="hHS" style="color:#ffd700">0m</div></div>
    <div class="hi"><div class="lb">Checkpoints</div><div class="vl" id="hC">0</div></div>
    <div class="hi"><div class="lb">2nd Jump</div><div class="vl" id="hDJ" style="color:#ff6aff">✦</div></div>
    <div class="hi" id="hudDash" style="display:none"><div class="lb">Dash [E]</div><div class="vl" id="hDash" style="color:#00e5ff">RDY</div></div>
    <div class="hi" id="hudFly" style="display:none"><div class="lb">Fly [E/B/↓]</div><div class="vl" style="color:#ffcc00">ON</div></div>
    <div class="hi" id="hudTp" style="display:none"><div class="lb">Teleport</div><div class="vl" style="color:#ff6aff">[Shift+L]</div></div>
  </div>
  <div id="cc">
    <canvas id="gc"></canvas>
    <div id="cf">✦ CHECKPOINT ✦</div>
    <div id="unlockPop"></div>
    <div id="voidTimer"></div>
    <div id="po">PAUSED</div>
  </div>
  <div id="tc">
    <div class="tr">
      <div class="ts">
        <div class="vb" id="tL">◀</div>
        <div class="vb" id="tR">▶</div>
      </div>
      <div class="vb jb" id="tJ">JUMP</div>
    </div>
  </div>
</div>
</div>

<script>
// ══════════════════════════════════════════════════════
//  GO UP!  —  endless vertical platformer
//
//  COORDINATE SYSTEM (clean, no patches):
//    World Y increases downward (canvas default).
//    Higher altitude = smaller worldY value.
//    cam = worldY value shown at screen TOP (y=0).
//    screenY = worldY - cam   →   helper: sc(worldY)
//    Camera follows player upward, never goes back down.
// ══════════════════════════════════════════════════════

// ── Colour helpers ────────────────────────────────────
function hexToRgb(hex){
  const r=parseInt(hex.slice(1,3),16),g=parseInt(hex.slice(3,5),16),b=parseInt(hex.slice(5,7),16);
  return[r,g,b];
}
function lighten(hex,amt){
  const[r,g,b]=hexToRgb(hex);
  return`rgb(${Math.min(255,r+Math.round(255*amt))},${Math.min(255,g+Math.round(255*amt))},${Math.min(255,b+Math.round(255*amt))})`;
}
function darken(hex,amt){
  const[r,g,b]=hexToRgb(hex);
  return`rgb(${Math.max(0,r-Math.round(255*amt))},${Math.max(0,g-Math.round(255*amt))},${Math.max(0,b-Math.round(255*amt))})`;
}

// safe arc helper
function A(c,x,y,r,s,e){c.arc(x,y,r<0?0:r,s,e)}

// ── Audio ─────────────────────────────────────────────
const AC=window.AudioContext||window.webkitAudioContext;
let ac=null;
function ea(){if(!ac)try{ac=new AC()}catch(e){}}
function beep(type,f0,f1,dur,vol){
  if(!ac)return;
  const o=ac.createOscillator(),g=ac.createGain();
  o.connect(g);g.connect(ac.destination);
  o.type=type;o.frequency.setValueAtTime(f0,ac.currentTime);
  if(f1)o.frequency.exponentialRampToValueAtTime(f1,ac.currentTime+dur*.8);
  g.gain.setValueAtTime(vol,ac.currentTime);
  g.gain.exponentialRampToValueAtTime(.001,ac.currentTime+dur);
  o.start();o.stop(ac.currentTime+dur+.02);
}
const sndJump=()=>beep('square',280,520,.18,.07);
const sndDJ  =()=>beep('square',460,760,.16,.08);
const sndDie =()=>beep('sawtooth',350,55,.6,.1);
const sndCkpt=()=>[0,.13,.26].forEach((t,i)=>setTimeout(()=>beep('sine',[440,550,660][i],null,.2,.09),t*1e3));

// ── Canvas ────────────────────────────────────────────
const cv=document.getElementById('gc');
const cx=cv.getContext('2d');

// ── Constants ─────────────────────────────────────────
const GRAV=.52, JF=-13.2, SPEED=4.5;
const PW=28, PH=28, PLH=14, TILE=32;

// ── State ─────────────────────────────────────────────
let S={}, running=false, paused=false, bestH=0, raf=null;
let highScore=parseInt(localStorage.getItem('goUpHigh')||'0');
let playerColor=localStorage.getItem('goUpColor')||'#7b2fff';
let bgColor=localStorage.getItem('goUpBg')||'#07050f';
let difficulty='medium';

const STYLES=[
  {id:'default',  name:'Void',       desc:'classic dark space',  cube:'#7b2fff', bg:'#07050f', card:'#0d0520'},
  {id:'ocean',    name:'Ocean',      desc:'deep blue vibes',      cube:'#00e5ff', bg:'#020d1a', card:'#031528'},
  {id:'forest',   name:'Forest',     desc:'cube in the trees',    cube:'#00ff88', bg:'#02100a', card:'#041a0e'},
  {id:'lava',     name:'Lava',       desc:'hot hot hot',          cube:'#ff6600', bg:'#110200', card:'#1a0400'},
  {id:'candy',    name:'Candy',      desc:'pastel dream',         cube:'#ff6aff', bg:'#180820', card:'#220c2e'},
  {id:'gold',     name:'Gold Rush',  desc:'dripping in it',       cube:'#ffcc00', bg:'#0d0900', card:'#1a1000'},
  {id:'ice',      name:'Ice',        desc:'cold and clean',       cube:'#aae4ff', bg:'#020d14', card:'#03111a'},
  {id:'blood',    name:'Blood Moon', desc:'red nightmare',        cube:'#ff2244', bg:'#0f0002', card:'#1a0005'},
  {id:'neon',     name:'Neon City',  desc:'cyberpunk skyline',    cube:'#ff00ff', bg:'#050008', card:'#0a000f'},
  {id:'sunset',   name:'Sunset',     desc:'orange skies',         cube:'#ff9944', bg:'#0d0604', card:'#1a0a06'},
]; // 'easy' | 'medium' | 'hard'

// ── Colour picker (disk) ───────────────────────────────
let wheelBrightness=0.5;
let wheelDotX=-1, wheelDotY=-1;
let wardRaf=null;

function buildWheel(dotX,dotY){
  const wc=document.getElementById('colWheel');
  const ww=wc.width, wh=wc.height;
  // wh is split: top 200px = disc, bottom 24px = brightness slider
  const discH=176, sliderH=24, sliderY=discH+4;
  const wx=wc.getContext('2d');
  wx.clearRect(0,0,ww,wh);
  const cx2=ww/2, cy2=discH/2, R=discH/2-6;
  const L=wheelBrightness;

  // ── draw disc pixel-by-pixel ──
  const img=wx.createImageData(ww,discH);
  for(let y=0;y<discH;y++){
    for(let x=0;x<ww;x++){
      const dx=x-cx2, dy=y-cy2;
      const dist=Math.sqrt(dx*dx+dy*dy);
      if(dist>R+1){ img.data[(y*ww+x)*4+3]=0; continue; }
      const hue=((Math.atan2(dy,dx)*180/Math.PI)+360)%360;
      const sat=Math.min(1,dist/R);
      const [r,g,b]=hslToRgb(hue/360,sat,L);
      const i=(y*ww+x)*4;
      img.data[i]=r; img.data[i+1]=g; img.data[i+2]=b; img.data[i+3]=255;
    }
  }
  wx.putImageData(img,0,0);

  // ── thick black outer ring ──
  wx.beginPath(); wx.arc(cx2,cy2,R+1,0,Math.PI*2);
  wx.arc(cx2,cy2,R+7,0,Math.PI*2,true);
  wx.fillStyle='#000'; wx.fill();
  // white border around outer ring
  wx.beginPath(); wx.arc(cx2,cy2,R+8,0,Math.PI*2);
  wx.strokeStyle='#333'; wx.lineWidth=1.5; wx.stroke();
  // inner ring subtle
  wx.beginPath(); wx.arc(cx2,cy2,R,0,Math.PI*2);
  wx.strokeStyle='rgba(255,255,255,.15)'; wx.lineWidth=1; wx.stroke();

  // ── brightness slider ──
  const sliderW=ww-20, sliderX=10;
  // gradient: black → full colour → white
  const sg=wx.createLinearGradient(sliderX,0,sliderX+sliderW,0);
  sg.addColorStop(0,'#000000');
  sg.addColorStop(.5,'hsl(0,100%,50%)'); // placeholder, will draw per-pixel... actually gradient is fine
  sg.addColorStop(1,'#ffffff');
  // draw a rainbow→brightness gradient: black → hue strip → white
  for(let i=0;i<=sliderW;i++){
    const t=i/sliderW; // 0=black,1=white
    const hue=0; // neutral, use grey
    const [r,g,b]=hslToRgb(0,0,t);
    wx.fillStyle=`rgb(${r},${g},${b})`;
    wx.fillRect(sliderX+i,sliderY,1,16);
  }
  // overlay hue sweep at brightness .5 — multiply with L
  // Instead: just draw black→saturated→white per position
  wx.clearRect(sliderX,sliderY,sliderW,16);
  for(let i=0;i<=sliderW;i++){
    const t=i/sliderW;
    // black(0) → vivid(0.5) → white(1)
    const [r,g,b]=hslToRgb(0,t<.5?1:1-(t-.5)*2, t<.5?t*2:.5+(t-.5));
    // simpler: interpolate black→vivid orange→white
    let r2,g2,b2;
    if(t<.5){ const f=t*2; r2=Math.round(f*255);g2=Math.round(f*80);b2=Math.round(f*255); }
    else { const f=(t-.5)*2; r2=Math.round(255-f*(255-255));g2=Math.round(80+f*175);b2=Math.round(255); }
    // just use HSL greyscale + colour hint
    const grey=Math.round(t*255);
    wx.fillStyle=`rgb(${grey},${grey},${grey})`;
    wx.fillRect(sliderX+i,sliderY,1,16);
  }
  // draw slider as proper black→grey→white gradient
  wx.clearRect(sliderX,sliderY,sliderW,16);
  const bwg=wx.createLinearGradient(sliderX,0,sliderX+sliderW,0);
  bwg.addColorStop(0,'#000');
  bwg.addColorStop(.15,'#1a1a1a');
  bwg.addColorStop(.35,'#555');
  bwg.addColorStop(.5,'hsl(260,80%,55%)');
  bwg.addColorStop(.65,'hsl(200,90%,65%)');
  bwg.addColorStop(.85,'#ffccee');
  bwg.addColorStop(1,'#fff');
  wx.fillStyle=bwg;
  wx.beginPath(); wx.roundRect(sliderX,sliderY,sliderW,16,8); wx.fill();
  wx.strokeStyle='#333'; wx.lineWidth=1;
  wx.beginPath(); wx.roundRect(sliderX,sliderY,sliderW,16,8); wx.stroke();

  // slider thumb
  const thumbX=sliderX+wheelBrightness*sliderW;
  wx.beginPath(); wx.arc(thumbX,sliderY+8,9,0,Math.PI*2);
  wx.fillStyle='#fff'; wx.fill();
  wx.strokeStyle='#222'; wx.lineWidth=2; wx.stroke();

  // ── dot on disc ──
  const dx2=dotX!=null&&dotX>=0?dotX:cx2;
  const dy2=dotX!=null&&dotY>=0?dotY:cy2;
  drawWheelDot(wx,dx2,dy2);

  // store slider geometry for hit detection
  wc._sliderX=sliderX; wc._sliderY=sliderY;
  wc._sliderW=sliderW; wc._sliderH=16;
  wc._discCX=cx2; wc._discCY=cy2; wc._discR=R;
  wc._discH=discH;
}

function hslToRgb(h,s,l){
  let r,g,b;
  if(s===0){r=g=b=l;}
  else{
    const hue2=q=>q<0?q+1:q>1?q-1:q;
    const hue3=(p,q,t)=>{
      if(t<1/6)return p+(q-p)*6*t;
      if(t<1/2)return q;
      if(t<2/3)return p+(q-p)*(2/3-t)*6;
      return p;
    };
    const q=l<.5?l*(1+s):l+s-l*s, p=2*l-q;
    r=hue3(p,q,hue2(h+1/3));
    g=hue3(p,q,hue2(h));
    b=hue3(p,q,hue2(h-1/3));
  }
  return[Math.round(r*255),Math.round(g*255),Math.round(b*255)];
}

function drawWheelDot(wx,x,y,w,h){
  wx.save();
  wx.beginPath(); wx.arc(x,y,8,0,Math.PI*2);
  wx.strokeStyle='#fff'; wx.lineWidth=2.5; wx.stroke();
  wx.beginPath(); wx.arc(x,y,5,0,Math.PI*2);
  wx.strokeStyle='#000'; wx.lineWidth=1; wx.stroke();
  wx.restore();
}

function wheelPick(e){
  const wc=document.getElementById('colWheel');
  const wx=wc.getContext('2d');
  const rect=wc.getBoundingClientRect();
  const scaleX=wc.width/rect.width, scaleY=wc.height/rect.height;
  const touch=e.touches?e.touches[0]:e;
  const x=(touch.clientX-rect.left)*scaleX;
  const y=(touch.clientY-rect.top)*scaleY;
  const{_sliderX,_sliderY,_sliderW,_sliderH,_discCX,_discCY,_discR,_discH}=wc;

  let changed=false;

  // hit slider?
  if(y>=_sliderY-6 && y<=_sliderY+_sliderH+6){
    wheelBrightness=Math.max(0,Math.min(1,(x-_sliderX)/_sliderW));
    changed=true;
  }

  // hit disc?
  const dx=x-_discCX, dy=y-_discCY, dist=Math.sqrt(dx*dx+dy*dy);
  if(dist<=_discR+10){
    const clampedDist=Math.min(dist,_discR);
    const cx2n=_discCX+dx/Math.max(dist,.001)*clampedDist;
    const cy2n=_discCY+dy/Math.max(dist,.001)*clampedDist;
    wheelDotX=cx2n; wheelDotY=cy2n;
    changed=true;
  }

  if(!changed)return;
  buildWheel(wheelDotX,wheelDotY);
  updateWardPreview();
}

function pickColorFromWheel(){
  const wc=document.getElementById('colWheel');
  const{_discCX,_discCY,_discR,_discH}=wc;
  if(!_discCX) return;
  const dx=wheelDotX<0?0:wheelDotX-_discCX;
  const dy=wheelDotY<0?0:wheelDotY-_discCY;
  const dist=Math.sqrt(dx*dx+dy*dy);
  const sat=Math.min(1,dist/(_discR||1));
  const rawAngle=Math.atan2(dy,dx);
  const hue=((rawAngle)*180/Math.PI%360+360)%360;
  const[r,g,b]=hslToRgb(hue/360,sat,wheelBrightness);
  return`#${[r,g,b].map(v=>v.toString(16).padStart(2,'0')).join('')}`;
}

function updateWardPreview(){
  const col=pickColorFromWheel();
  if(!col)return;
  document.getElementById('wardPreview').style.background=col;
  document.getElementById('wardPreview').style.boxShadow=`0 0 18px ${col}`;
  document.getElementById('wardHex').textContent=col;
  playerColor=col;
}

function showWard(){
  document.getElementById('scrStart').classList.add('hidden');
  document.getElementById('scrWard').classList.remove('hidden');
  wheelDotX=-1; wheelDotY=-1;
  const[r,g,b]=hexToRgb(playerColor);
  wheelBrightness=Math.max(r,g,b)/255*.7+.15;
  buildWheel(-1,-1);
  document.getElementById('wardPreview').style.background=playerColor;
  document.getElementById('wardPreview').style.boxShadow=`0 0 18px ${playerColor}`;
  document.getElementById('wardHex').textContent=playerColor;
  const wc=document.getElementById('colWheel');
  wc.addEventListener('mousedown',wheelPick);
  wc.addEventListener('mousemove',e=>{if(e.buttons)wheelPick(e)});
  wc.addEventListener('touchstart',wheelPick,{passive:true});
  wc.addEventListener('touchmove',wheelPick,{passive:true});

  // static wheel — no animation
  buildWheel(wheelDotX, wheelDotY);
}

// ── Input ─────────────────────────────────────────────
const K={left:false,right:false,jump:false,dash:false};
let jEdge=false, dashEdge=false;
let adminMode=false, ownerMode=false;

window.addEventListener('keydown',e=>{
  if(document.getElementById('redeemModal').style.display==='flex') return;
  if(document.getElementById('tpModal').style.display==='flex') return;
  if(['ArrowLeft','a','A'].includes(e.key))K.left=true;
  if(['ArrowRight','d','D'].includes(e.key))K.right=true;
  if(['ArrowUp','w','W',' '].includes(e.key)){if(!K.jump)jEdge=true;K.jump=true;}
  if(['e','E','b','B','PageDown','q','Q','c','C','PageUp'].includes(e.key)){if(!K.dash)dashEdge=true; K.dash=true; e.preventDefault();}
  if(['p','P'].includes(e.key))togglePause();
  if(e.key==='Escape'){if(running){running=false;if(raf)cancelAnimationFrame(raf);showStart();}}
  if(e.key==='L'&&e.shiftKey&&ownerMode&&running){e.preventDefault();openTpModal();}
  if([' ','ArrowUp','ArrowDown','ArrowLeft','ArrowRight'].includes(e.key))e.preventDefault();
});
window.addEventListener('keyup',e=>{
  if(['ArrowLeft','a','A'].includes(e.key))K.left=false;
  if(['ArrowRight','d','D'].includes(e.key))K.right=false;
  if(['ArrowUp','w','W',' '].includes(e.key))K.jump=false;
  if(['e','E','b','B','PageDown','q','Q','c','C','PageUp'].includes(e.key))K.dash=false;
});
function mkTouch(id,key){
  const el=document.getElementById(id);
  const on=e=>{e.preventDefault();K[key]=true;if(key==='jump')jEdge=true;el.classList.add('active')};
  const off=e=>{e.preventDefault();K[key]=false;el.classList.remove('active')};
  el.addEventListener('touchstart',on,{passive:false});
  ['touchend','touchcancel'].forEach(v=>el.addEventListener(v,off,{passive:false}));
  el.addEventListener('mousedown',on);
  ['mouseup','mouseleave'].forEach(v=>el.addEventListener(v,off));
}
mkTouch('tL','left'); mkTouch('tR','right'); mkTouch('tJ','jump');

// ── Resize ────────────────────────────────────────────
function resize(){
  const c=document.getElementById('cc');
  cv.width=c.clientWidth; cv.height=c.clientHeight;
}
window.addEventListener('resize',resize);

// world → screen
function sc(worldY){return worldY-S.cam}

// ── Platform factory ──────────────────────────────────
// type: 'normal' | 'crumble' | 'ckpt'
function mkPlat(x,y,w,type){
  const isMoving = type==='moving';
  return{
    x,y,w,h:PLH,type,
    // checkpoint state
    done:false,
    // crumble state
    stepped:false, fallTimer:0, falling:false, fallVy:0, alpha:1,
    // moving platform state
    moveDir: isMoving ? (Math.random()<.5?1:-1) : 0,
    moveSpd: isMoving ? 1.2+Math.random()*1.4 : 0,
    moveMin: 0,
    moveMax: 0,  // set to W-w after creation
    // visual
    sh:Math.random()*Math.PI*2, cs:Math.random()
  };
}

function genPlats(startY,count,W){
  const out=[];
  let y=startY;
  const isEasy=difficulty==='easy';
  const isHard=difficulty==='hard';
  for(let i=0;i<count;i++){
    const baseW=isEasy?90:isHard?50:60;
    const varW=isEasy?60:isHard?55:80;
    const w=baseW+Math.random()*varW;
    const x=Math.random()*Math.max(1,W-w);
    let type='normal';
    // checkpoints: easy + medium only, not hard
    if(!isHard && i>3 && i%9===0) type='ckpt';
    // crumble: none on easy, 15% medium, 30% hard
    const crumbleChance=isEasy?0:isHard?.30:.15;
    if(i>4 && type==='normal' && Math.random()<crumbleChance) type='crumble';
    // moving: 22% easy, 18% medium, 10% hard
    const movingChance=isEasy?.22:isHard?.10:.18;
    if(i>6 && type==='normal' && Math.random()<movingChance) type='moving';
    const pl=mkPlat(x,y,w,type);
    pl.idx=(S.platCount||0)+i+1;
    if(type==='moving'){
      pl.moveMin=0;
      pl.moveMax=W-w;
      pl.moveSpd=isEasy?0.8+Math.random()*0.5:isHard?2.0+Math.random()*1.5:1.2+Math.random()*1.4;
      if(isEasy){ pl.w=Math.max(pl.w,110); pl.moveMax=Math.max(0,W-pl.w); }
    }
    out.push(pl);
    y -= TILE*1.6 + Math.random()*TILE*1.1;
  }
  return out;
}

// ── Background elements ───────────────────────────────
function mkStars(W,H){
  return Array.from({length:140},()=>({
    x:Math.random()*W, y:Math.random()*(H*14),
    r:Math.random()*1.8+.2, a:Math.random(),
    px:Math.random()<.4?.28:1
  }));
}
function mkNebs(W,H){
  return Array.from({length:10},()=>({
    x:Math.random()*W, y:Math.random()*(H*14),
    rx:50+Math.random()*130, ry:25+Math.random()*70,
    hue:Math.random()<.5?220+Math.random()*40:270+Math.random()*50,
    alpha:.04+Math.random()*.07
  }));
}

// ── Init game ─────────────────────────────────────────
function initGame(){
  resize();
  const W=cv.width, H=cv.height;

  let plats, px, py, groundY;

  if(difficulty==='hard'){
    // hardcore: no spawn platform — drop from above first platform
    const genned=genPlats(H-60, 90, W);
    plats=genned;
    const firstPlat=genned[0];
    px=firstPlat.x+firstPlat.w/2-PW/2;
    py=firstPlat.y-PH-80; // spawn above first platform
    groundY=firstPlat.y;
  } else {
    // easy/medium: solid ground platform to start on
    const ground=mkPlat(W/2-60, H-60, 120, 'normal');
    ground.idx=0;
    const extraPlats=genPlats(ground.y-TILE*2.5, 90, W);
    plats=[ground, ...extraPlats];
    px=ground.x+ground.w/2-PW/2;
    py=ground.y-PH;
    groundY=ground.y;
  }

  S={
    W, H,
    plats,
    stars: mkStars(W,H),
    nebs:  mkNebs(W,H),
    cam: py - H*.55,
    player:{
      x:px, y:py,
      vx:0, vy:0,
      onGround:false, wasOnGround:false,
      coyote:0, jbuf:0,
      airJumps:1, maxAirJumps:1,
      dead:false, dTimer:0, inVoid:false, voidTimer:0, fallTime:0,
      trail:[], facingR:true,
      squish:1, sqTimer:0, glow:0
    },
    particles:[],
    score:0, ckpts:0, visited:1,
    platCount: 90,
    spawnX:px, spawnY:py,
    groundY: groundY,
    time:0, flash:0,
    unlockedTriple:false, unlockedDash:false,
    dashCooldown:0,
    flyEarned:0
  };
}

// ── Extend world upward ───────────────────────────────
function extendWorld(){
  const{plats,W,H}=S;
  const top=plats[plats.length-1];
  // generate more when top platform is within 50% of screen from cam
  if(top.y > S.cam - H*.4){
    const newPlats=genPlats(top.y-TILE*2, 25, W);
    S.platCount+=25;
    plats.push(...newPlats);
  }
  // prune platforms below camera — keep checkpoints and spawn platform always
  S.plats=S.plats.filter(pl=>pl.idx===0 || pl.type==='ckpt' || pl.y < S.cam+S.H*1.5);
}

// ── Particles ─────────────────────────────────────────
function spawnP(x,y,col,n,o={}){
  for(let i=0;i<n;i++){
    const a=o.cone!=null ? o.cone+(Math.random()-.5)*(o.spread||Math.PI*2) : Math.random()*Math.PI*2;
    const spd=(o.ms||1)+Math.random()*(o.xs||4);
    S.particles.push({
      x,y, vx:Math.cos(a)*spd, vy:Math.sin(a)*spd-(o.up||0),
      life:1, decay:o.dc||(0.025+Math.random()*.04),
      r:(o.r||2)+Math.random()*(o.xr||4), col, glow:!!o.glow
    });
  }
}
function tickP(dt){
  S.particles=S.particles.filter(p=>p.life>0);
  S.particles.forEach(p=>{p.x+=p.vx;p.y+=p.vy;p.vy+=.13;p.life-=p.decay});
}

// ── Update ────────────────────────────────────────────
function update(dt){
  if(paused)return;
  S.time+=dt;
  const p=S.player;

  // ── dead: wait then respawn ──
  if(p.dead){
    p.dTimer-=dt;
    S.flash=Math.max(0,S.flash-dt*2.4);
    if(p.dTimer<=0)respawn();
    tickP(dt);
    return;
  }

  // continuous falling → die after 2.5s in any mode
  if(p.vy > 1 && !p.onGround){
    p.fallTime=(p.fallTime||0)+dt/60;
    if(p.fallTime>=2.5){
      const vEl=document.getElementById('voidTimer');
      const left=2.5-(p.fallTime-dt/60); // show last moment
      vEl.textContent='💀 FALLING…'; vEl.classList.add('show');
      vEl.classList.remove('show'); killPlayer(); return;
    } else if(p.fallTime>=1.0){
      const vEl=document.getElementById('voidTimer');
      const secs=Math.ceil(2.5-p.fallTime);
      vEl.textContent='💀 '+secs+'…'; vEl.classList.add('show');
    }
  } else {
    p.fallTime=0;
    document.getElementById('voidTimer').classList.remove('show');
  }

  // ── unlock checks ──
  if(!S.unlockedTriple && S.visited>=75){
    S.unlockedTriple=true;
    showUnlock('triple','✦ TRIPLE JUMP UNLOCKED ✦','jump 3 times in the air!');
  }
  if(!S.unlockedDash && S.visited>=150){
    S.unlockedDash=true;
    document.getElementById('hudDash').style.display='';
    showUnlock('dash','⚡ DASH UNLOCKED ✦','Q / C / PageUp');
  }

  // ── dash cooldown tick ──
  if(S.dashCooldown>0){
    S.dashCooldown=Math.max(0,S.dashCooldown-dt/60);
  }

  // ── dash input (one-shot on press edge) ──
  if(dashEdge){
    dashEdge=false;
    if(!adminMode && !ownerMode && S.unlockedDash && S.dashCooldown<=0){
      p.vy=JF*2.8;
      p.squish=1.7; p.sqTimer=12; p.glow=2.5;
      S.dashCooldown=20;
      beep('square',700,1100,.12,.12);
      spawnP(p.x+PW/2,p.y+PH/2,'#00e5ff',18,{cone:-Math.PI/2,spread:Math.PI*.8,up:3,ms:3,xs:7,r:3,xr:5,glow:true});
      spawnP(p.x+PW/2,p.y+PH,'#ffffff',10,{cone:-Math.PI/2,spread:.6,up:4,ms:2,xs:5,r:1.5,xr:3});
    }
  }

  // ── coyote time ──
  if(p.onGround)  p.coyote=10;
  else if(p.coyote>0) p.coyote--;

  // ── jump buffer ──
  if(jEdge)       p.jbuf=8;
  else if(p.jbuf>0) p.jbuf--;

  // ── jump (supports triple jump when unlocked) ──
  if(p.jbuf>0){
    if(p.coyote>0){
      // normal / coyote jump
      p.vy=JF; p.onGround=false;
      p.coyote=0; p.jbuf=0;
      p.squish=1.45; p.sqTimer=9; p.glow=1;
      sndJump();
      spawnP(p.x+PW/2,p.y+PH,'#00e5ff',7,{cone:-Math.PI/2,spread:.8,up:2,ms:1.5,xs:4,r:2,xr:4,glow:true});
    } else if(p.airJumps>0){
      const isTriple=S.unlockedTriple && p.maxAirJumps===2 && p.airJumps===1;
      p.airJumps--;
      p.vy=JF*.88; p.jbuf=0;
      p.squish=1.5; p.sqTimer=10; p.glow=1.4;
      if(isTriple){
        // triple jump — gold starburst
        beep('square',560,900,.16,.09);
        spawnP(p.x+PW/2,p.y+PH/2,'#ffcc00',16,{cone:-Math.PI/2,spread:Math.PI*1.5,up:1,ms:2,xs:6,r:2.5,xr:5,glow:true});
        spawnP(p.x+PW/2,p.y+PH/2,'#fff',6,{cone:-Math.PI/2,spread:Math.PI*.5,up:3,ms:3,xs:4,r:1,xr:2});
      } else {
        sndDJ();
        spawnP(p.x+PW/2,p.y+PH/2,'#ff6aff',12,{cone:-Math.PI/2,spread:Math.PI*1.2,up:1,ms:2,xs:5,r:2,xr:4,glow:true});
        spawnP(p.x+PW/2,p.y+PH/2,'#fff',5,{cone:-Math.PI/2,spread:Math.PI*.6,up:2,ms:3,xs:4,r:1,xr:2});
      }
    }
  }
  jEdge=false;

  // ── horizontal movement ──
  const dir=(K.left?-1:0)+(K.right?1:0);
  if(dir){ p.vx=dir*SPEED; p.facingR=dir>0; }
  else p.vx*=.76;

  // ── gravity / fly ──
  if((adminMode||ownerMode) && K.dash){
    // fly: hold E / B / PageDown to float up
    p.vy=Math.max(p.vy-1.2,-9);
    p.onGround=false;
    p.glow=Math.max(p.glow,0.6);
    // score: count every ~TILE*1.6 px of upward flight as 1 platform
    const PLAT_H=TILE*1.6+TILE*0.55; // avg gap between platforms
    if(!p.flyBaseY) p.flyBaseY=p.y;
    const risen=p.flyBaseY-p.y;
    const earned=Math.floor(risen/PLAT_H);
    if(earned>=(S.flyEarned||0)+1){
      S.flyEarned=(S.flyEarned||0)+1;
      S.visited++;
    }
  } else {
    if(p.flyBaseY) p.flyBaseY=p.y; // reset baseline when not flying
    p.vy=Math.min(p.vy+GRAV, 22);
  }

  // ── move ──
  p.x+=p.vx; p.y+=p.vy;
  if(p.x+PW<0) p.x=S.W;
  if(p.x>S.W)  p.x=-PW;

  // ── squish / glow decay ──
  if(p.sqTimer>0){ p.sqTimer--; p.squish=1+.45*(p.sqTimer/9)*(p.squish>=1?1:-1); }
  else p.squish+=(1-p.squish)*.22;
  p.glow=Math.max(0,p.glow-.04);

  // ── platform collision ──
  p.wasOnGround=p.onGround;
  p.onGround=false;

  for(const pl of S.plats){
    if(pl.falling) continue;
    // cull
    if(p.y+PH < pl.y-80 || p.y > pl.y+pl.h+20) continue;
    // land from above
    if(p.vy>=0 &&
       p.x+PW > pl.x && p.x < pl.x+pl.w &&
       p.y+PH > pl.y  && p.y+PH < pl.y+pl.h+Math.abs(p.vy)+2){

      p.y=pl.y-PH;
      p.vy=0;
      p.onGround=true;
      p.maxAirJumps=S.unlockedTriple?2:1;
      p.airJumps=p.maxAirJumps;

      if(!p.wasOnGround && !pl.visited){
        pl.visited=true;
        S.visited++;
        p.squish=.65; p.sqTimer=7;
        spawnP(p.x+PW/2,pl.y,'#00e5ff',4,{cone:0,spread:Math.PI,up:-1,ms:1,xs:2,r:1.5,xr:2.5});
      } else if(!p.wasOnGround){
        p.squish=.65; p.sqTimer=7;
        spawnP(p.x+PW/2,pl.y,'#00e5ff',4,{cone:0,spread:Math.PI,up:-1,ms:1,xs:2,r:1.5,xr:2.5});
      }

      // start crumble
      if(pl.type==='crumble' && !pl.stepped){
        pl.stepped=true;
        pl.fallTimer=0.45;
      }

      // checkpoint
      if(pl.type==='ckpt' && !pl.done){
        pl.done=true;
        S.spawnX=pl.x+pl.w/2-PW/2;
        S.spawnY=pl.y-PH;
        S.ckpts++;
        sndCkpt(); flashCkpt();
        spawnP(p.x+PW/2, p.y+PH/2, '#00ff88', 22,
          {ms:2,xs:6,up:2,r:2,xr:5,glow:true});
      }
    }
  }

  // ── moving platform tick ──
  for(const pl of S.plats){
    if(pl.type!=='moving') continue;
    const prev=pl.x;
    pl.x+=pl.moveDir*pl.moveSpd;
    if(pl.x<=pl.moveMin){pl.x=pl.moveMin;pl.moveDir=1;}
    if(pl.x+pl.w>=pl.moveMax+pl.w){pl.x=pl.moveMax;pl.moveDir=-1;}
    // carry player if standing on this platform
    if(p.onGround && p.y+PH>=pl.y && p.y+PH<=pl.y+pl.h+2 &&
       p.x+PW>pl.x && p.x<pl.x+pl.w){
      p.x+=pl.x-prev;
    }
  }

  // ── crumble platform tick ──
  for(const pl of S.plats){
    if(pl.type!=='crumble') continue;
    if(pl.falling){
      pl.fallVy+=.55; pl.y+=pl.fallVy;
      pl.alpha=Math.max(0,pl.alpha-.016);
      continue;
    }
    if(pl.stepped){
      pl.fallTimer -= dt/60;
      if(pl.fallTimer<=0){
        pl.falling=true; pl.fallVy=1.2;
        for(let i=0;i<10;i++)
          spawnP(pl.x+Math.random()*pl.w, pl.y,
            i%2?'#ff8800':'#cc4400', 1,
            {cone:.1,spread:Math.PI*.8,up:-1,ms:1,xs:4,r:2,xr:4});
      }
    }
  }
  S.plats=S.plats.filter(pl=>!(pl.falling&&pl.alpha<=0));

  // ── trail ──
  p.trail.push({x:p.x+PW/2, y:p.y+PH/2, life:1});
  if(p.trail.length>14) p.trail.shift();
  p.trail.forEach(t=>t.life-=.1);

  // ── score: unique platforms visited ──
  if(S.visited>highScore){ highScore=S.visited; localStorage.setItem('goUpHigh',highScore); }

  // ── camera: smoothly follow player (both directions for respawn) ──
  const targetCam=p.y-S.H*.55;
  S.cam+=(targetCam-S.cam)*.09;

  extendWorld();
  tickP(dt);

  // ── HUD ──
  document.getElementById('hH').textContent=S.visited+' pl';
  document.getElementById('hB').textContent='—';
  document.getElementById('hHS').textContent=highScore+' pl';
  document.getElementById('hC').textContent=S.ckpts;
  const djEl=document.getElementById('hDJ');
  const maxJ=p.maxAirJumps||1;
  const dots='✦'.repeat(p.airJumps)+'·'.repeat(Math.max(0,maxJ-p.airJumps));
  djEl.textContent=p.airJumps>0?dots+' RDY':dots+' used';
  djEl.style.color=p.airJumps>0?'#ff6aff':'#333';
  // dash HUD
  if(S.unlockedDash){
    document.getElementById('hudDash').style.display='';
    const dashEl=document.getElementById('hDash');
    if(S.dashCooldown>0){
      dashEl.textContent=Math.ceil(S.dashCooldown)+'s';
      dashEl.style.color='#ff4444';
    } else {
      dashEl.textContent='RDY ⚡';
      dashEl.style.color='#00e5ff';
    }
  }
  if(adminMode||ownerMode) document.getElementById('hudFly').style.display='';
  if(ownerMode) document.getElementById('hudTp').style.display='';
}

// ── Kill / respawn ─────────────────────────────────────
function killPlayer(){
  const p=S.player; if(p.dead)return;
  p.dead=true; p.dTimer=55; S.flash=1; sndDie();
  spawnP(p.x+PW/2,p.y+PH/2,'#ff6aff',25,{ms:2,xs:6,up:2,r:2,xr:5,glow:true});
  spawnP(p.x+PW/2,p.y+PH/2,'#fff',8,{ms:4,xs:8,r:1.5,xr:2.5});
}
function respawn(){
  const p=S.player;
  if(difficulty==='hard'){ showOver(); return; }
  p.x=S.spawnX; p.y=S.spawnY;
  p.vx=0; p.vy=0; p.dead=false; p.dTimer=0;
  p.onGround=false; p.trail=[]; p.glow=0;
  p.maxAirJumps=S.unlockedTriple?2:1;
  p.airJumps=p.maxAirJumps;
  p.inVoid=false; p.fallTime=0;
  S.cam=S.spawnY - S.H*.55;
  document.getElementById('voidTimer').classList.remove('show');
}

// ══════════════════════════════════════════════════════
//  DRAW
// ══════════════════════════════════════════════════════
function draw(){
  const{W,H,plats,stars,nebs,particles}=S;
  const p=S.player;
  cx.clearRect(0,0,W,H);

  // 1. background
  cx.fillStyle=bgColor; cx.fillRect(0,0,W,H);
  const bg=cx.createLinearGradient(0,0,0,H);
  bg.addColorStop(0,bgColor);
  bg.addColorStop(.6,bgColor);
  bg.addColorStop(1,bgColor);
  cx.fillStyle=bg; cx.fillRect(0,0,W,H);

  const TH=H*14;

  // 2. nebulae (parallax .22x)
  cx.save();
  nebs.forEach(n=>{
    const sy=((n.y-S.cam*.22)%TH+TH)%TH-H;
    if(sy<-250||sy>H+250)return;
    const rr=Math.max(1,n.rx);
    cx.save(); cx.scale(1,n.ry/n.rx);
    const g2=cx.createRadialGradient(n.x,sy*(n.rx/n.ry),0,n.x,sy*(n.rx/n.ry),rr);
    g2.addColorStop(0,`hsla(${n.hue},60%,34%,${n.alpha})`);
    g2.addColorStop(.6,`hsla(${n.hue},40%,17%,${n.alpha*.4})`);
    g2.addColorStop(1,'rgba(0,0,0,0)');
    cx.fillStyle=g2; cx.beginPath(); A(cx,n.x,sy*(n.rx/n.ry),rr,0,Math.PI*2); cx.fill();
    cx.restore();
  });
  cx.restore();

  // 3. grid (parallax .14x)
  cx.save(); cx.globalAlpha=.04; cx.strokeStyle='#4422cc'; cx.lineWidth=.5;
  const gs=48;
  const goy=(((-S.cam*.14)%gs)+gs)%gs;
  const gox=(((S.time*.3)%gs)+gs)%gs;
  for(let x=gox-gs;x<W+gs;x+=gs){cx.beginPath();cx.moveTo(x,0);cx.lineTo(x,H);cx.stroke()}
  for(let y=goy;y<H+gs;y+=gs){cx.beginPath();cx.moveTo(0,y);cx.lineTo(W,y);cx.stroke()}
  cx.restore();

  // 4. stars
  cx.save();
  stars.forEach(s=>{
    const sy=((s.y-S.cam*s.px)%TH+TH)%TH-H;
    if(sy<-5||sy>H+5)return;
    const fl=.4+.6*Math.sin(S.time*.025+s.a*12);
    const r=s.px<.5?s.r*.5:s.r; if(r<.05)return;
    cx.globalAlpha=(s.px<.5?.3:.85)*fl;
    if(s.r>1.5&&s.px>=1){
      cx.strokeStyle=`rgba(180,200,255,${.18*fl})`; cx.lineWidth=.5;
      cx.beginPath(); cx.moveTo(s.x-r*3,sy); cx.lineTo(s.x+r*3,sy); cx.stroke();
      cx.beginPath(); cx.moveTo(s.x,sy-r*3); cx.lineTo(s.x,sy+r*3); cx.stroke();
    }
    cx.fillStyle='rgba(215,225,255,1)';
    cx.beginPath(); A(cx,s.x,sy,r,0,Math.PI*2); cx.fill();
  });
  cx.globalAlpha=1; cx.restore();

  // 5. particles
  cx.save();
  particles.forEach(pt=>{
    if(pt.life<=0)return;
    const sy=sc(pt.y);
    cx.globalAlpha=Math.max(0,pt.life);
    if(pt.glow){cx.shadowBlur=12;cx.shadowColor=pt.col}
    cx.fillStyle=pt.col;
    cx.beginPath(); A(cx,pt.x,sy,Math.max(0,pt.r*pt.life),0,Math.PI*2); cx.fill();
    cx.shadowBlur=0;
  });
  cx.globalAlpha=1; cx.restore();

  // 6. platforms
  plats.forEach(pl=>{
    const sy=sc(pl.y);
    if(sy>H+50||sy+pl.h<-60)return;
    const sh=.5+.5*Math.sin(S.time*.05+pl.sh);
    cx.save();
    if(pl.alpha<1)cx.globalAlpha=pl.alpha;

    // crumble shake (last 1.2s)
    if(pl.type==='crumble'&&pl.stepped&&!pl.falling&&pl.fallTimer<0.45){
      const prog=1-pl.fallTimer/0.45;
      cx.translate((Math.random()-.5)*prog*7, 0);
    }

    if(pl.type==='ckpt'&&!pl.done){
      // ── CHECKPOINT — glowing green ──
      const aR=sh*18;
      const aura=cx.createRadialGradient(pl.x+pl.w/2,sy+pl.h/2,0,pl.x+pl.w/2,sy+pl.h/2,Math.max(1,pl.w/2+aR));
      aura.addColorStop(0,`rgba(0,255,136,${.2*sh})`);
      aura.addColorStop(.7,`rgba(0,200,100,${.06*sh})`);
      aura.addColorStop(1,'rgba(0,200,100,0)');
      cx.fillStyle=aura; cx.fillRect(pl.x-aR*2,sy-aR*2,pl.w+aR*4,pl.h+aR*4);
      const pg=cx.createLinearGradient(pl.x,sy,pl.x,sy+pl.h);
      pg.addColorStop(0,`rgb(0,${Math.floor(210+45*sh)},${Math.floor(110+60*sh)})`);
      pg.addColorStop(1,'#003d1e');
      cx.fillStyle=pg; cx.shadowBlur=18+10*sh; cx.shadowColor='#00ff88';
      cx.beginPath(); cx.roundRect(pl.x,sy,pl.w,pl.h,4); cx.fill();
      cx.strokeStyle=`rgba(80,255,160,${.5+.4*sh})`; cx.lineWidth=1.8; cx.shadowBlur=0;
      cx.beginPath(); cx.roundRect(pl.x,sy,pl.w,pl.h,4); cx.stroke();
      const hi=cx.createLinearGradient(pl.x,sy,pl.x+pl.w,sy);
      hi.addColorStop(0,'rgba(255,255,255,0)'); hi.addColorStop(.4,`rgba(180,255,200,${.4+.3*sh})`); hi.addColorStop(1,'rgba(255,255,255,0)');
      cx.fillStyle=hi; cx.fillRect(pl.x+2,sy,pl.w-4,3);

    } else if(pl.type==='ckpt'&&pl.done){
      // ── activated checkpoint — dim grey-green ──
      cx.fillStyle='#1a2a1a';
      cx.beginPath(); cx.roundRect(pl.x,sy,pl.w,pl.h,4); cx.fill();

    } else if(pl.type==='crumble'){
      // ── CRUMBLE — orange→red, cracks appear ──
      const danger=pl.stepped ? Math.max(0,1-pl.fallTimer/0.45) : 0;
      const pg=cx.createLinearGradient(pl.x,sy,pl.x,sy+pl.h);
      pg.addColorStop(0,`rgb(${Math.floor(200+55*sh)},${Math.floor(80-65*danger)},20)`);
      pg.addColorStop(1,`rgb(${Math.floor(110+30*sh)},${Math.floor(35-25*danger)},8)`);
      cx.fillStyle=pg; cx.shadowBlur=8+danger*14; cx.shadowColor=danger>.5?'#ff2200':'#ff8800';
      cx.beginPath(); cx.roundRect(pl.x,sy,pl.w,pl.h,4); cx.fill(); cx.shadowBlur=0;
      // top highlight
      const hi=cx.createLinearGradient(pl.x,sy,pl.x+pl.w,sy);
      hi.addColorStop(0,'rgba(255,255,255,0)'); hi.addColorStop(.4,`rgba(255,210,100,${.35+.2*sh})`); hi.addColorStop(1,'rgba(255,255,255,0)');
      cx.fillStyle=hi; cx.fillRect(pl.x+2,sy,pl.w-4,3);
      // cracks after stepping
      if(danger>.15){
        cx.strokeStyle=`rgba(255,50,0,${danger*.7})`; cx.lineWidth=1;
        cx.beginPath(); cx.moveTo(pl.x+pl.w*.3,sy+2); cx.lineTo(pl.x+pl.w*.3+5,sy+pl.h-1); cx.stroke();
        cx.beginPath(); cx.moveTo(pl.x+pl.w*.66,sy+1); cx.lineTo(pl.x+pl.w*.66-4,sy+pl.h-1); cx.stroke();
      }
      // warning icon
      if(pl.stepped&&!pl.falling){
        cx.globalAlpha=(pl.alpha||1)*(.55+.45*Math.sin(S.time*(2+danger*7)));
        cx.fillStyle=`rgb(255,${Math.floor(185-110*danger)},0)`;
        cx.font=`bold ${Math.floor(11+danger*4)}px sans-serif`;
        cx.textAlign='center'; cx.textBaseline='bottom';
        cx.fillText('⚠',pl.x+pl.w/2,sy-3);
      }

    } else {
      // ── MOVING — cyan/teal with directional arrows ──
      if(pl.type==='moving'){
      const mg=cx.createLinearGradient(pl.x,sy,pl.x,sy+pl.h);
      mg.addColorStop(0,`rgb(0,${Math.floor(210+30*sh)},${Math.floor(180+40*sh)})`);
      mg.addColorStop(.45,'#006b6b');
      mg.addColorStop(1,'#002222');
      cx.fillStyle=mg; cx.shadowBlur=10+7*sh; cx.shadowColor='#00ffee';
      cx.beginPath(); cx.roundRect(pl.x,sy,pl.w,pl.h,4); cx.fill();
      cx.strokeStyle=`rgba(0,255,238,${.4+.3*sh})`; cx.lineWidth=1.5; cx.shadowBlur=0;
      cx.beginPath(); cx.roundRect(pl.x,sy,pl.w,pl.h,4); cx.stroke();
      // top highlight
      const mhi=cx.createLinearGradient(pl.x,sy,pl.x+pl.w,sy);
      mhi.addColorStop(0,'rgba(255,255,255,0)'); mhi.addColorStop(.4,`rgba(180,255,250,${.4+.3*sh})`); mhi.addColorStop(1,'rgba(255,255,255,0)');
      cx.fillStyle=mhi; cx.fillRect(pl.x+2,sy,pl.w-4,3);
      // moving arrows
      const arrowAlpha=.45+.35*Math.sin(S.time*.12+pl.sh);
      cx.fillStyle=`rgba(0,255,238,${arrowAlpha})`;
      cx.font=`bold 9px sans-serif`; cx.textAlign='center'; cx.textBaseline='middle';
      const arrow=pl.moveDir>0?'▶':'◀';
      cx.fillText(arrow, pl.x+pl.w/2-6, sy+pl.h/2);
      cx.fillText(arrow, pl.x+pl.w/2+6, sy+pl.h/2);
      } else {
      // ── NORMAL — purple/blue neon ──
      const hue=220+pl.cs*45;
      const pg=cx.createLinearGradient(pl.x,sy,pl.x,sy+pl.h);
      pg.addColorStop(0,`hsla(${hue},88%,${Math.floor(52+22*sh)}%,1)`);
      pg.addColorStop(.42,`hsla(${hue},72%,34%,1)`);
      pg.addColorStop(1,`hsla(${hue},65%,12%,1)`);
      cx.fillStyle=pg; cx.shadowBlur=8+6*sh; cx.shadowColor=`hsla(${hue},100%,68%,.4)`;
      cx.beginPath(); cx.roundRect(pl.x,sy,pl.w,pl.h,4); cx.fill();
      const hi=cx.createLinearGradient(pl.x,sy,pl.x+pl.w,sy);
      hi.addColorStop(0,'rgba(255,255,255,0)'); hi.addColorStop(.35,`rgba(255,255,255,${.3+.2*sh})`);
      hi.addColorStop(.72,`rgba(255,255,255,${.16+.1*sh})`); hi.addColorStop(1,'rgba(255,255,255,0)');
      cx.fillStyle=hi; cx.shadowBlur=0; cx.fillRect(pl.x+2,sy,pl.w-4,3);
      }
    }
    cx.restore();
  });

  // 7. player trail
  p.trail.forEach(t=>{
    if(t.life<=0)return;
    cx.save(); cx.globalAlpha=Math.max(0,t.life*.24);
    cx.fillStyle=playerColor; cx.shadowBlur=8; cx.shadowColor=playerColor;
    cx.beginPath(); A(cx,t.x,sc(t.y),Math.max(0,(PW/2)*t.life),0,Math.PI*2); cx.fill();
    cx.restore();
  });

  // 8. player
  if(!p.dead||Math.floor(S.time*2)%2===0){
    const px2=p.x, py2=sc(p.y);
    const cx2=px2+PW/2, cy2=py2+PH/2;
    cx.save();
    cx.translate(cx2,cy2);
    cx.scale(1/Math.max(.1,p.squish), p.squish);
    cx.translate(-PW/2,-PH/2);
    cx.shadowBlur=18+p.glow*28; cx.shadowColor=playerColor;
    // body gradient — derive lighter/darker shades from chosen colour
    const bG=cx.createLinearGradient(0,0,PW,PH);
    bG.addColorStop(0,lighten(playerColor,.38));
    bG.addColorStop(.28,lighten(playerColor,.12));
    bG.addColorStop(.72,playerColor);
    bG.addColorStop(1,darken(playerColor,.35));
    cx.fillStyle=bG; cx.beginPath(); cx.roundRect(0,0,PW,PH,5); cx.fill();
    // inner ambient
    const iG=cx.createRadialGradient(PW*.38,PH*.3,0,PW/2,PH/2,PW*.7);
    iG.addColorStop(0,'rgba(220,200,255,.33)'); iG.addColorStop(1,'rgba(60,0,120,0)');
    cx.fillStyle=iG; cx.shadowBlur=0; cx.beginPath(); cx.roundRect(0,0,PW,PH,5); cx.fill();
    // border
    cx.strokeStyle='rgba(200,160,255,.45)'; cx.lineWidth=1.4;
    cx.beginPath(); cx.roundRect(.7,.7,PW-1.4,PH-1.4,4.5); cx.stroke();
    // eyes
    const ey=PH*.33, eo=p.facingR?1.2:-1.2;
    cx.fillStyle='rgba(255,255,255,.94)';
    cx.beginPath(); A(cx,PW*.33,ey,3.2,0,Math.PI*2); cx.fill();
    cx.beginPath(); A(cx,PW*.67,ey,3.2,0,Math.PI*2); cx.fill();
    cx.fillStyle='#1a0040';
    cx.beginPath(); A(cx,PW*.33+eo,ey+1,1.8,0,Math.PI*2); cx.fill();
    cx.beginPath(); A(cx,PW*.67+eo,ey+1,1.8,0,Math.PI*2); cx.fill();
    cx.fillStyle='rgba(255,255,255,.78)';
    cx.beginPath(); A(cx,PW*.33+eo-.6,ey+.2,.7,0,Math.PI*2); cx.fill();
    cx.beginPath(); A(cx,PW*.67+eo-.6,ey+.2,.7,0,Math.PI*2); cx.fill();
    // shine
    const shG=cx.createLinearGradient(3,2,PW*.7,PH*.4);
    shG.addColorStop(0,'rgba(255,255,255,.48)'); shG.addColorStop(1,'rgba(255,255,255,0)');
    cx.fillStyle=shG; cx.beginPath(); cx.roundRect(3,2,PW*.52,PH*.3,2); cx.fill();
    cx.restore();
  }

  // 9. death flash
  if(S.flash>0){
    cx.save(); cx.globalAlpha=Math.max(0,S.flash*.35);
    const df=cx.createRadialGradient(W/2,H/2,0,W/2,H/2,Math.max(W,H)*.85);
    df.addColorStop(0,'#7b2fff'); df.addColorStop(1,'rgba(60,0,120,0)');
    cx.fillStyle=df; cx.fillRect(0,0,W,H); cx.restore();
    S.flash=Math.max(0,S.flash-.04);
  }
}

// ── Checkpoint flash ──────────────────────────────────
let cfTO=null;
let _unlockTimer=null;
function showUnlock(type, title, sub){
  const el=document.getElementById('unlockPop');
  el.className='show '+type;
  el.innerHTML=title+'<div class="dash-cd">'+sub+'</div>';
  if(_unlockTimer)clearTimeout(_unlockTimer);
  // animate in then out
  el.style.animation='none';
  el.style.transition='opacity .3s, transform .3s';
  el.style.transform='translateX(-50%) translateY(0)';
  el.style.opacity='1';
  _unlockTimer=setTimeout(()=>{
    el.style.opacity='0';
    el.style.transform='translateX(-50%) translateY(-18px)';
    setTimeout(()=>{ el.className=''; },350);
  }, 3500);
}

function flashCkpt(){
  const el=document.getElementById('cf');
  el.classList.add('show'); clearTimeout(cfTO);
  cfTO=setTimeout(()=>el.classList.remove('show'),1800);
}

// ── Pause ─────────────────────────────────────────────
function togglePause(){
  paused=!paused;
  document.getElementById('po').classList.toggle('on',paused);
}

// ── Loop ─────────────────────────────────────────────
let lastTS=0;
function loop(ts){
  const dt=Math.min((ts-lastTS)/16.67,3); lastTS=ts;
  update(dt); draw();
  if(running) raf=requestAnimationFrame(loop);
}

// ── Screens ───────────────────────────────────────────
function applyBg(){
  // darken the bg slightly for the radial gradient overlay
  const[r,g,b]=hexToRgb(bgColor);
  const lighter=`rgb(${Math.min(255,r+30)},${Math.min(255,g+20)},${Math.min(255,b+50)})`;
  const sbgGrad=`radial-gradient(ellipse at 50% 55%,${lighter} 0%,${bgColor} 70%)`;
  document.body.style.background=bgColor;
  document.getElementById('app').style.background=bgColor;
  // update all .sbg overlay divs
  document.querySelectorAll('.sbg').forEach(el=>el.style.background=sbgGrad);
  // screen backgrounds
  ['scrWard','scrDiff','scrStart','scrOver','scrStyle'].forEach(id=>{
    const el=document.getElementById(id);
    if(el) el.style.background=bgColor;
  });
  // canvas container
  const cc=document.getElementById('cc');
  if(cc) cc.style.background=bgColor;
}

function showStyle(){
  document.getElementById('scrStart').classList.add('hidden');
  document.getElementById('scrStyle').classList.remove('hidden');
  const grid=document.getElementById('styleGrid');
  grid.innerHTML='';
  const saved=localStorage.getItem('goUpStyle')||'default';
  STYLES.forEach(s=>{
    const card=document.createElement('div');
    card.className='scard'+(s.id===saved?' active':'');
    card.style.background=s.card;
    card.style.borderColor=s.id===saved?'#fff':'rgba(255,255,255,.08)';
    card.innerHTML=`
      <div class="scard-cube" style="background:${s.cube};box-shadow:0 0 10px ${s.cube}88"></div>
      <div class="scard-info">
        <div class="scard-name">${s.name}</div>
        <div class="scard-desc">${s.desc}</div>
      </div>`;
    card.addEventListener('click',()=>{
      playerColor=s.cube; bgColor=s.bg;
      localStorage.setItem('goUpColor',playerColor);
      localStorage.setItem('goUpBg',bgColor);
      localStorage.setItem('goUpStyle',s.id);
      applyBg();
      grid.querySelectorAll('.scard').forEach(c=>{c.classList.remove('active');c.style.borderColor='rgba(255,255,255,.08)'});
      card.classList.add('active'); card.style.borderColor='#fff';
    });
    grid.appendChild(card);
  });
}

function showDiff(){
  document.getElementById('scrStart').classList.add('hidden');
  document.getElementById('scrDiff').classList.remove('hidden');
}
function hideDiff(){
  document.getElementById('scrDiff').classList.add('hidden');
  document.getElementById('scrStart').classList.remove('hidden');
}
function showStart(){
  document.getElementById('scrStart').classList.remove('hidden');
  document.getElementById('scrOver').classList.add('hidden');
  document.getElementById('scrWard').classList.add('hidden');
  document.getElementById('scrDiff').classList.add('hidden');
  document.getElementById('scrStyle').classList.add('hidden');
  document.getElementById('gw').style.display='none';
  running=false; if(raf)cancelAnimationFrame(raf);
  applyBg();
}
function showGame(){
  document.getElementById('scrStart').classList.add('hidden');
  document.getElementById('scrOver').classList.add('hidden');
  document.getElementById('scrDiff').classList.add('hidden');
  document.getElementById('gw').style.display='flex';
  paused=false; document.getElementById('po').classList.remove('on');
  initGame(); running=true; lastTS=performance.now();
  raf=requestAnimationFrame(loop);
}
function showOver(){
  running=false; if(raf)cancelAnimationFrame(raf);
  document.getElementById('txScore').textContent=S.visited+' platforms';
  document.getElementById('txHigh').textContent=highScore+' platforms';
  document.getElementById('txHigh').textContent=highScore+' platforms';
  document.getElementById('gw').style.display='none';
  document.getElementById('scrOver').classList.remove('hidden');
}

// ── Boot ─────────────────────────────────────────────
document.getElementById('btnPlay').addEventListener('click',()=>{ea();showDiff()});
document.getElementById('btnRestart').addEventListener('click',()=>{ea();showDiff()});
document.getElementById('dEasy').addEventListener('click',()=>{difficulty='easy';showGame()});
document.getElementById('dMed').addEventListener('click',()=>{difficulty='medium';showGame()});
document.getElementById('dHard').addEventListener('click',()=>{difficulty='hard';showGame()});
document.getElementById('diffBack').addEventListener('click',()=>hideDiff());
document.getElementById('btnWard').addEventListener('click',()=>showWard());
document.getElementById('btnStyle').addEventListener('click',()=>showStyle());
document.getElementById('btnStyleBack').addEventListener('click',()=>{
  document.getElementById('scrStyle').classList.add('hidden');
  document.getElementById('scrStart').classList.remove('hidden');
});
document.getElementById('btnWardSave').addEventListener('click',()=>{
  if(wardRaf){cancelAnimationFrame(wardRaf);wardRaf=null;}
  localStorage.setItem('goUpColor',playerColor);
  document.getElementById('scrWard').classList.add('hidden');
  document.getElementById('scrStart').classList.remove('hidden');
});

// ── Redeem codes ───────────────────────────────────────
function applyAdminPerks(){
  adminMode=true;
  if(running && S.player){
    S.unlockedTriple=true; S.unlockedDash=true;
    S.dashCooldown=0;
    S.player.maxAirJumps=2; S.player.airJumps=2;
    document.getElementById('hudDash').style.display='';
    document.getElementById('hudFly').style.display='';
    showUnlock('dash','⚡ ADMIN UNLOCKED','fly + triple jump + dash active');
  }
}
function applyOwnerPerks(){
  ownerMode=true; adminMode=true;
  if(running && S.player){
    S.unlockedTriple=true; S.unlockedDash=true;
    S.dashCooldown=0;
    S.player.maxAirJumps=2; S.player.airJumps=2;
    document.getElementById('hudDash').style.display='';
    document.getElementById('hudFly').style.display='';
    document.getElementById('hudTp').style.display='';
    showUnlock('dash','👑 OWNER UNLOCKED','all powers + Shift+L to teleport');
  }
}

function openRedeemModal(){
  const m=document.getElementById('redeemModal');
  m.style.display='flex';
  document.getElementById('redeemInput').value='';
  document.getElementById('redeemMsg').textContent='';
  document.getElementById('redeemMsg').className='';
  setTimeout(()=>document.getElementById('redeemInput').focus(),50);
}
function closeRedeemModal(){ document.getElementById('redeemModal').style.display='none'; }

function tryRedeem(){
  const raw=document.getElementById('redeemInput').value.trim();
  const code=raw.toLowerCase();
  const msg=document.getElementById('redeemMsg');
  if(code==='tdvadmin'){
    if(adminMode&&!ownerMode){ msg.className='msg-already'; msg.textContent='already redeemed!'; return; }
    applyAdminPerks();
    msg.className='msg-ok';
    msg.textContent='✓ admin unlocked — fly, triple jump & dash!';
    setTimeout(closeRedeemModal, 1800);
  } else if(code==='tdv[owner]'){
    if(ownerMode){ msg.className='msg-already'; msg.textContent='already redeemed!'; return; }
    applyOwnerPerks();
    msg.className='msg-ok';
    msg.textContent='✓ owner unlocked — all features + teleport!';
    setTimeout(closeRedeemModal, 1800);
  } else {
    msg.className='msg-err';
    msg.textContent='✗ invalid code';
  }
}

document.getElementById('btnRedeem').addEventListener('click',openRedeemModal);
document.getElementById('redeemClose').addEventListener('click',closeRedeemModal);
document.getElementById('redeemOk').addEventListener('click',tryRedeem);
document.getElementById('redeemInput').addEventListener('keydown',e=>{ if(e.key==='Enter')tryRedeem(); if(e.key==='Escape')closeRedeemModal(); });

// ── Teleport modal (Owner only) ──────────────────────────
function openTpModal(){
  if(!running) return;
  const wasPaused=paused;
  if(!wasPaused) togglePause();
  const m=document.getElementById('tpModal');
  m.style.display='flex';
  m.dataset.wasPaused=wasPaused?'1':'0';
  document.getElementById('tpInput').value='';
  document.getElementById('tpMsg').textContent='';
  document.getElementById('tpMsg').className='';
  setTimeout(()=>document.getElementById('tpInput').focus(),50);
}
function closeTpModal(){
  const m=document.getElementById('tpModal');
  m.style.display='none';
  if(m.dataset.wasPaused!=='1' && paused) togglePause();
}
function doTeleport(){
  const n=parseInt(document.getElementById('tpInput').value);
  const msg=document.getElementById('tpMsg');
  if(isNaN(n)||n<1){ msg.className='msg-err'; msg.textContent='enter a valid platform number'; return; }

  // ensure enough platforms exist — generate until we have n
  while(S.plats.filter(p=>p.type!=='ckpt'||true).length < n+10){
    const top=S.plats[S.plats.length-1];
    const newPs=genPlats(top.y-TILE*2,25,S.W);
    newPs.forEach(p=>{S.platCount++;p.idx=S.platCount;});
    S.plats.push(...newPs);
  }

  // find platform by idx
  const target=S.plats.find(p=>p.idx===n) || S.plats.sort((a,b)=>a.idx-b.idx)[Math.min(n-1,S.plats.length-1)];
  if(!target){ msg.className='msg-err'; msg.textContent='platform not found — try a smaller number'; return; }

  const p=S.player;
  p.x=target.x+target.w/2-PW/2;
  p.y=target.y-PH-2;
  p.vx=0; p.vy=0; p.fallTime=0;
  S.cam=p.y-S.H*.45;
  msg.className='msg-ok';
  msg.textContent='✓ teleported to platform '+n;
  setTimeout(closeTpModal, 900);
}

document.getElementById('tpClose').addEventListener('click',closeTpModal);
document.getElementById('tpOk').addEventListener('click',doTeleport);
document.getElementById('tpInput').addEventListener('keydown',e=>{ if(e.key==='Enter')doTeleport(); if(e.key==='Escape')closeTpModal(); });

document.addEventListener('touchstart',()=>ea(),{once:true});
applyBg();
showStart();
</script>
</body>
</html>
