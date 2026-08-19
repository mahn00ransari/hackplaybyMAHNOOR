# hackplaybyMAHNOOR
for game development


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<!-- Viewport tag for Mobile Responsiveness -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>SYSTEM_ERROR.exe</title>
<style>
:root{
  --void:      #07070d;   
  --panel:     #0d0f1a;   
  --panel-2:   #131629;   
  --line:      #23273f;   
  --cyan:      #4ff2ff;   
  --magenta:   #ff2ee0;   
  --amber:     #ffc857;   
  --text:      #cfe8ff;
  --text-dim:  #6d7699;
  --font-mono: 'Share Tech Mono', 'Courier New', monospace;
  --font-display: 'Orbitron', var(--font-mono);
}

@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@500;700&display=swap');

*{box-sizing:border-box; margin:0; padding:0;}
html,body{height:100%;}
body{
  background:var(--void);
  color:var(--text);
  font-family:var(--font-mono);
  overflow:hidden;
  height:100vh;
  position:relative;
  cursor:default;
  -webkit-font-smoothing:antialiased;
}

body::before{
  content:'';
  position:fixed; inset:0;
  background-image:
    linear-gradient(rgba(79,242,255,0.035) 1px, transparent 1px),
    linear-gradient(90deg, rgba(79,242,255,0.035) 1px, transparent 1px);
  background-size:40px 40px;
  pointer-events:none;
  z-index:0;
}

.scanlines{
  position:fixed; inset:0;
  pointer-events:none;
  z-index:9999;
  background:repeating-linear-gradient(
    to bottom,
    rgba(0,0,0,0) 0px,
    rgba(0,0,0,0) 2px,
    rgba(0,0,0,0.12) 3px
  );
  mix-blend-mode:overlay;
}
.vignette{
  position:fixed; inset:0;
  pointer-events:none;
  z-index:9998;
  box-shadow: inset 0 0 180px rgba(0,0,0,0.85);
}

/* BOOT SCREEN (MOBILE OPTIMIZED) */
#boot{
  position:fixed; inset:0;
  background:var(--void);
  display:flex; flex-direction:column;
  align-items:flex-start; justify-content:center;
  padding:20px;
  z-index:100;
  transition:opacity 0.8s ease;
  overflow-y:auto;
}
#boot-lines{ font-size:14px; line-height:1.7; color:var(--cyan); white-space:pre-wrap; }
#boot-lines .dim{ color:var(--text-dim); }
#boot-login{
  margin-top:28px; display:none; flex-wrap:wrap; align-items:center; gap:10px; width:100%;
}
#boot-login.show{ display:flex; }
#boot-login label{ color:var(--amber); font-size:13px; letter-spacing:1px; }
#boot-login input{
  background:transparent;
  border:none;
  border-bottom:2px solid var(--cyan);
  color:var(--cyan);
  font-family:var(--font-mono);
  font-size:16px;
  padding:4px 6px;
  outline:none;
  flex:1;
  min-width:150px;
  letter-spacing:1px;
}
#boot-login button{
  background:transparent;
  border:1px solid var(--cyan);
  color:var(--cyan);
  font-family:var(--font-mono);
  padding:6px 14px;
  cursor:pointer;
  letter-spacing:2px;
  transition:all 0.15s;
}
#boot-login button:hover{ background:var(--cyan); color:var(--void); }

/* DESKTOP & ICON GRID (RESPONSIVE GRID FOR MOBILE) */
#desktop{
  position:fixed; inset:0;
  z-index:1;
  display:none;
  padding:16px 16px 50px 16px;
  overflow-y:auto;
}
#desktop.show{ display:block; }

.icon-grid{
  display:grid;
  grid-template-columns:repeat(auto-fill, minmax(85px, 1fr));
  gap:16px;
  width:100%;
}
.icon{
  display:flex; flex-direction:column; align-items:center;
  gap:6px;
  cursor:pointer;
  opacity:0;
  transform:scale(0.7);
  animation:icon-pop 0.5s forwards;
  user-select:none;
  -webkit-tap-highlight-color: transparent;
}
@keyframes icon-pop{
  0%{ opacity:0; transform:scale(0.6) rotate(-4deg); filter:blur(4px); }
  60%{ opacity:1; transform:scale(1.08) rotate(1deg); filter:blur(0); }
  100%{ opacity:1; transform:scale(1) rotate(0); }
}
.icon .glyph{
  width:50px; height:50px;
  border:1px solid var(--line);
  background:var(--panel);
  display:flex; align-items:center; justify-content:center;
  font-family:var(--font-display);
  font-size:18px;
  color:var(--cyan);
  text-shadow:0 0 8px rgba(79,242,255,0.6);
  transition:all 0.15s;
}
.icon:hover .glyph, .icon:active .glyph{
  border-color:var(--cyan);
  box-shadow:0 0 16px rgba(79,242,255,0.35);
  transform:translateY(-2px);
}
.icon.corrupt .glyph{ color:var(--magenta); text-shadow:0 0 10px rgba(255,46,224,0.7); border-color:rgba(255,46,224,0.4); }
.icon .label{
  font-size:11px; text-align:center; color:var(--text-dim);
  max-width:85px; line-height:1.2; word-break:break-word;
}

/* TASKBAR */
#taskbar{
  position:fixed; left:0; right:0; bottom:0;
  height:36px;
  background:var(--panel);
  border-top:1px solid var(--line);
  display:flex; align-items:center; justify-content:space-between;
  padding:0 12px;
  font-size:11px;
  color:var(--text-dim);
  z-index:50;
  letter-spacing:0.5px;
}
#taskbar .path::before{ content:'C:\\USERS\\'; color:var(--text-dim); }
#taskbar-user{ color:var(--cyan); }
#taskbar-app{ color:var(--amber); }

/* TOAST NOTIFICATIONS */
#toast-wrap{
  position:fixed; top:12px; right:12px; left:12px; z-index:200;
  display:flex; flex-direction:column; gap:8px; align-items:flex-end;
  pointer-events:none;
}
.toast{
  background:var(--panel);
  border:1px solid var(--cyan);
  color:var(--cyan);
  padding:8px 12px;
  font-size:11px;
  letter-spacing:0.5px;
  box-shadow:0 0 14px rgba(79,242,255,0.25);
  animation:toast-in 0.3s ease, toast-out 0.4s ease 2.6s forwards;
  max-width:100%;
}
@keyframes toast-in{ from{ transform:translateX(30px); opacity:0;} to{transform:translateX(0); opacity:1;} }
@keyframes toast-out{ to{ transform:translateX(30px); opacity:0;} }

/* WINDOWS (MOBILE RESPONSIVE SIZING) */
#window-layer{
  position:fixed; inset:0;
  z-index:40;
  display:none;
  align-items:center; justify-content:center;
  background:rgba(3,4,8,0.7);
  backdrop-filter:blur(3px);
  padding:12px;
}
#window-layer.show{ display:flex; }

.win{
  width:min(640px, 100%);
  max-height:85vh;
  background:var(--panel);
  border:1px solid var(--line);
  box-shadow:0 0 40px rgba(0,0,0,0.6);
  display:flex; flex-direction:column;
  animation:win-open 0.25s ease;
}
@keyframes win-open{ from{ opacity:0; transform:scale(0.92);} to{opacity:1; transform:scale(1);} }
.win-bar{
  display:flex; align-items:center; justify-content:space-between;
  padding:9px 12px;
  background:var(--panel-2);
  border-bottom:1px solid var(--line);
  font-size:11px;
  letter-spacing:1px;
  color:var(--cyan);
}
.win-bar .close{
  cursor:pointer; color:var(--text-dim); font-size:16px; padding:0 6px;
}
.win-bar .close:hover{ color:var(--magenta); }
.win-body{
  padding:16px;
  overflow-y:auto;
  font-size:13px;
  line-height:1.6;
  color:var(--text);
}
.win-body::-webkit-scrollbar{ width:4px; }
.win-body::-webkit-scrollbar-thumb{ background:var(--line); }

.term-line{ white-space:pre-wrap; color:var(--cyan); word-break:break-word; }
.term-line.dim{ color:var(--text-dim); }
.term-line.warn{ color:var(--magenta); }

.file-row{
  display:flex; align-items:center; gap:10px;
  padding:10px 8px;
  border-bottom:1px solid var(--line);
  cursor:pointer;
  font-size:13px;
}
.file-row:hover{ background:var(--panel-2); }
.file-row .tag{
  margin-left:auto; font-size:10px; color:var(--text-dim); letter-spacing:1px;
}
.file-row.locked .tag{ color:var(--magenta); }

.pw-input{
  margin-top:16px; display:flex; gap:8px; flex-wrap:wrap;
}
.pw-input input{
  flex:1; min-width:140px; background:var(--void); border:1px solid var(--line);
  color:var(--cyan); font-family:var(--font-mono); padding:9px 10px; outline:none; font-size:14px;
}
.pw-input input:focus{ border-color:var(--cyan); }
.pw-input button{
  background:transparent; border:1px solid var(--cyan); color:var(--cyan);
  padding:9px 16px; cursor:pointer; letter-spacing:1px;
}
.pw-input button:hover{ background:var(--cyan); color:var(--void); }
.shake{ animation:shake 0.4s; }
@keyframes shake{ 20%,60%{transform:translateX(-8px);} 40%,80%{transform:translateX(8px);} }

.btn{
  display:inline-block; margin-top:16px;
  background:transparent; border:1px solid var(--cyan); color:var(--cyan);
  padding:9px 18px; cursor:pointer; letter-spacing:1px; font-family:var(--font-mono);
  font-size:12px; transition:all .15s; text-align:center;
}
.btn:hover, .btn:active{ background:var(--cyan); color:var(--void); }
.btn.danger{ border-color:var(--magenta); color:var(--magenta); }
.btn.danger:hover, .btn.danger:active{ background:var(--magenta); color:var(--void); }

/* VIDEO STATIC */
.static-box{
  height:150px; position:relative; overflow:hidden;
  background:#000; border:1px solid var(--line);
  display:flex; align-items:center; justify-content:center;
}
.static-box canvas{ position:absolute; inset:0; width:100%; height:100%; opacity:0.5; }
.static-flash{
  position:relative; z-index:2;
  font-family:var(--font-display);
  font-size:22px; letter-spacing:2px;
  color:var(--magenta);
  text-shadow:0 0 12px rgba(255,46,224,0.8);
  text-align:center;
}
.captured-line{
  margin-top:14px; font-size:12px; color:var(--amber); letter-spacing:1px; min-height:20px;
}

/* BROWSER WINDOW */
.browser-bar{
  display:flex; align-items:center; gap:8px;
  background:var(--void); border:1px solid var(--line);
  padding:7px 10px; margin:-4px 0 16px 0; font-size:11px; color:var(--text-dim);
  overflow-x:auto;
}
.eye-art{
  font-size:11px; line-height:1.3; color:var(--magenta); text-align:center;
  text-shadow:0 0 10px rgba(255,46,224,0.6);
  margin:10px 0 4px 0;
}

/* ENDING SCREEN */
#ending{
  position:fixed; inset:0; z-index:500;
  background:#000; display:none;
  align-items:center; justify-content:center;
  flex-direction:column; text-align:center; padding:20px;
  overflow-y:auto;
}
#ending.show{ display:flex; }
#ending .line{
  font-family:var(--font-display);
  color:var(--magenta);
  font-size:clamp(15px, 4vw, 24px);
  letter-spacing:1px;
  margin:8px 0;
  text-shadow:0 0 18px rgba(255,46,224,0.7);
}
#ending .sub{
  font-family:var(--font-mono); color:var(--text-dim); font-size:12px; margin-top:20px; max-width:560px; line-height:1.7;
}
#ending-choices{ margin-top:24px; display:flex; gap:12px; flex-wrap:wrap; justify-content:center; }
.glitch-hit{ animation:glitch-hit 0.5s steps(2); }
@keyframes glitch-hit{
  0%{ transform:translate(0);}
  20%{ transform:translate(-6px,3px); filter:hue-rotate(90deg);}
  40%{ transform:translate(6px,-2px); filter:hue-rotate(-60deg);}
  60%{ transform:translate(-3px,-3px);}
  80%{ transform:translate(3px,2px);}
  100%{ transform:translate(0); filter:none;}
}
.flicker{ animation:flicker 2.2s infinite; }
@keyframes flicker{
  0%,100%{ opacity:1; } 92%{opacity:1;} 93%{opacity:0.3;} 94%{opacity:1;} 96%{opacity:0.5;} 97%{opacity:1;}
}
</style>
</head>
<body>

<div class="scanlines"></div>
<div class="vignette"></div>
<div id="toast-wrap"></div>

<!-- BOOT -->
<div id="boot">
  <div id="boot-lines"></div>
  <div id="boot-login">
    <label>USERNAME:</label>
    <input id="name-input" maxlength="16" autocomplete="off" spellcheck="false">
    <button id="login-btn">LOG IN</button>
  </div>
</div>

<!-- DESKTOP -->
<div id="desktop">
  <div class="icon-grid" id="icon-grid"></div>
  <div id="taskbar">
    <span class="path">USERS\<span id="taskbar-user"></span>&gt; <span id="taskbar-app">desktop</span></span>
    <span id="clock"></span>
  </div>
</div>

<!-- WINDOW LAYER -->
<div id="window-layer">
  <div class="win" id="win-box">
    <div class="win-bar">
      <span id="win-title">WINDOW</span>
      <span class="close" id="win-close">✕</span>
    </div>
    <div class="win-body" id="win-body"></div>
  </div>
</div>

<!-- ENDING -->
<div id="ending"></div>

<script>
const state = {
  name: 'USER',
  stage: 0,          
  unlocked: { sysError:true, video:false, docs:false, hidden:false, site:false },
  solvedPassword: false
};

const ICONS = [
  { id:'sysError', label:'SYSTEM_ERROR.exe', glyph:'[!]', corrupt:true,  open:openSysError },
  { id:'video',    label:'corrupted_clip.mp4', glyph:'[▶]', corrupt:false, open:openVideo },
  { id:'docs',     label:'Documents',        glyph:'[/]', corrupt:false, open:openDocs },
  { id:'hidden',   label:'.sys_hidden',      glyph:'[?]', corrupt:true,  open:openHidden },
  { id:'site',     label:'sys-net.err.url',  glyph:'[~]', corrupt:false, open:openSite },
];

function $(sel){ return document.querySelector(sel); }
function el(tag, cls, html){ const e=document.createElement(tag); if(cls) e.className=cls; if(html!==undefined) e.innerHTML=html; return e; }

function toast(msg){
  const t = el('div','toast', msg);
  $('#toast-wrap').appendChild(t);
  setTimeout(()=>t.remove(), 3200);
}

function typeLines(container, lines, speed, done){
  let i = 0;
  function next(){
    if(i >= lines.length){ if(done) done(); return; }
    const [text, cls] = Array.isArray(lines[i]) ? lines[i] : [lines[i], ''];
    const line = el('div', 'term-line ' + (cls||''));
    container.appendChild(line);
    let c = 0;
    const iv = setInterval(()=>{
      line.textContent = text.slice(0, c+1);
      c++;
      if(c >= text.length){
        clearInterval(iv);
        i++;
        setTimeout(next, 380);
      }
    }, speed || 18);
  }
  next();
}

function scrambleReveal(node, finalText, duration){
  const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ01#$%&';
  const len = finalText.length;
  const start = performance.now();
  duration = duration || 900;
  function frame(now){
    const t = Math.min(1, (now-start)/duration);
    let out = '';
    for(let i=0;i<len;i++){
      if(finalText[i] === ' '){ out += ' '; continue; }
      if(t > i/len) out += finalText[i];
      else out += chars[Math.floor(Math.random()*chars.length)];
    }
    node.textContent = out;
    if(t < 1) requestAnimationFrame(frame);
    else node.textContent = finalText;
  }
  requestAnimationFrame(frame);
}

const bootLines = $('#boot-lines');
typeLines(bootLines, [
  ['BIOS CHECK ................. OK', 'dim'],
  ['LOADING KERNEL .............. OK', 'dim'],
  ['MOUNTING USER SHELL ......... OK', 'dim'],
  ['UNKNOWN PROCESS DETECTED: SYSTEM_ERROR.exe', 'warn'],
  ['origin: unresolved', 'dim'],
  ['this file was not installed by you.', 'warn'],
], 14, () => {
  $('#boot-login').classList.add('show');
  $('#name-input').focus();
});

function login(){
  const v = $('#name-input').value.trim();
  state.name = v || 'USER';
  $('#boot').style.opacity = '0';
  setTimeout(()=>{
    $('#boot').style.display='none';
    $('#desktop').classList.add('show');
    $('#taskbar-user').textContent = state.name.toUpperCase();
    renderIcons();
  }, 800);
}
$('#login-btn').addEventListener('click', login);
$('#name-input').addEventListener('keydown', e=>{ if(e.key==='Enter') login(); });

setInterval(()=>{
  const d = new Date();
  $('#clock').textContent = d.toLocaleTimeString();
}, 1000);

function renderIcons(){
  const grid = $('#icon-grid');
  grid.innerHTML = '';
  ICONS.forEach((ic, idx)=>{
    if(!state.unlocked[ic.id]) return;
    const wrap = el('div','icon'+(ic.corrupt?' corrupt':''));
    wrap.style.animationDelay = (idx*0.05)+'s';
    wrap.innerHTML = `<div class="glyph">${ic.glyph}</div><div class="label">${ic.label}</div>`;
    wrap.addEventListener('click', ic.open);
    grid.appendChild(wrap);
  });
}

function unlock(id, toastMsg){
  if(state.unlocked[id]) return;
  state.unlocked[id] = true;
  renderIcons();
  if(toastMsg) toast(toastMsg);
}

function openWindow(title, appName){
  $('#window-layer').classList.add('show');
  $('#win-title').textContent = title;
  $('#taskbar-app').textContent = appName || title.toLowerCase();
  const body = $('#win-body');
  body.innerHTML = '';
  return body;
}
function closeWindow(){
  $('#window-layer').classList.remove('show');
  $('#taskbar-app').textContent = 'desktop';
}
$('#win-close').addEventListener('click', closeWindow);

function openSysError(){
  const body = openWindow('SYSTEM_ERROR.exe — terminal', 'system_error.exe');
  const term = el('div');
  body.appendChild(term);
  typeLines(term, [
    ['> booting SYSTEM_ERROR.exe ...', ''],
    ['> ERROR: file has no verified origin', 'warn'],
    ['> ERROR: created before this machine was purchased', 'warn'],
    ['> ...hello, ' + state.name + '.', ''],
    ['> did you know this program has been idle since before you owned this laptop?', 'dim'],
    ['> I found something in the temp cache. Check the desktop.', ''],
  ], 16, ()=>{
    unlock('video', 'NEW FILE DETECTED: corrupted_clip.mp4');
  });
}

function openVideo(){
  const body = openWindow('corrupted_clip.mp4 — media player', 'corrupted_clip.mp4');
  body.innerHTML = `
    <div class="static-box">
      <canvas id="static-canvas"></canvas>
      <div class="static-flash" id="flash-word"></div>
    </div>
    <div class="captured-line" id="captured-line">frame buffer: ______________</div>
  `;
  const canvas = $('#static-canvas');
  const ctx = canvas.getContext('2d');
  canvas.width = 300; canvas.height = 120;
  const staticIv = setInterval(()=>{
    const img = ctx.createImageData(canvas.width, canvas.height);
    for(let i=0;i<img.data.length;i+=4){
      const v = Math.random()*255;
      img.data[i]=v; img.data[i+1]=v; img.data[i+2]=v; img.data[i+3]=255;
    }
    ctx.putImageData(img,0,0);
  }, 60);

  const decoys = ['░░░░','#ERR','0x4F','????','▓▓▓▓','NULL','—̷—̷'];
  const reveal = ['LOOK','IN','DOCUMENTS'];
  const flashEl = $('#flash-word');
  const capturedEl = $('#captured-line');
  let captured = '';
  let step = 0;

  const flashIv = setInterval(()=>{
    if(step < reveal.length*4){
      const cycle = step % 4;
      if(cycle === 3){
        flashEl.textContent = reveal[Math.floor(step/4)];
        flashEl.style.color = 'var(--cyan)';
      } else {
        flashEl.textContent = decoys[Math.floor(Math.random()*decoys.length)];
        flashEl.style.color = 'var(--magenta)';
      }
      step++;
    } else {
      clearInterval(flashIv);
      clearInterval(staticIv);
      captured = reveal.join(' ');
      capturedEl.textContent = 'frame buffer: ' + captured;
      capturedEl.style.color = 'var(--cyan)';
      flashEl.textContent = '';
      const btn = el('div','btn','OPEN DOCUMENTS');
      btn.addEventListener('click', ()=>{
        unlock('docs', 'NEW FOLDER DETECTED: Documents');
        closeWindow();
      });
      body.appendChild(btn);
    }
  }, 180);
}

function openDocs(){
  const body = openWindow('Documents', 'documents');
  body.innerHTML = `
    <div class="file-row" id="file-diary">
      <span>📄 diary.txt</span><span class="tag">readable</span>
    </div>
    <div class="file-row" id="file-resume">
      <span>📄 resume_DRAFT2_FINAL_v3.docx</span><span class="tag">readable</span>
    </div>
    <div class="file-row locked" id="file-pw">
      <span>🔒 passwords.txt</span><span class="tag">${state.solvedPassword ? 'unlocked' : 'locked'}</span>
    </div>
  `;
  $('#file-diary').addEventListener('click', openDiary);
  $('#file-resume').addEventListener('click', openResume);
  $('#file-pw').addEventListener('click', openPasswordFile);
}

function openDiary(){
  const body = openWindow('diary.txt', 'diary.txt');
  body.innerHTML = `
    <div style="color:var(--text-dim); font-size:12px; margin-bottom:10px;">Entry #47</div>
    <div>I can't remember installing this program. But it knows my schedule. It knows when I sleep.</div>
    <div style="margin-top:10px;">It keeps asking for a word. I think I already know what it wants.</div>
    <div style="margin-top:16px; color:var(--amber); border-left:2px solid var(--amber); padding-left:12px;">
      "I move through your files, unseen but always near.<br>
      Five letters. I haunt machines the way I haunt old houses."
    </div>
    <div style="margin-top:16px;">I think the answer is obvious. I just don't want to type it.</div>
  `;
}

function openResume(){
  const body = openWindow('resume_DRAFT2_FINAL_v3.docx', 'resume.docx');
  body.innerHTML = `
    <div style="color:var(--text-dim);">— objective —</div>
    <div style="margin-top:8px;">To find a job before this laptop finds me first.</div>
    <div style="margin-top:16px; color:var(--text-dim);">— skills —</div>
    <div style="margin-top:8px;">Pretending to understand CSS. Closing 30 browser tabs at once. Ignoring update prompts.</div>
    <div style="margin-top:16px; font-size:11px; color:var(--text-dim);">(this file does nothing. it's just a draft. probably.)</div>
  `;
}

function openPasswordFile(){
  const body = openWindow('passwords.txt', 'passwords.txt');
  if(state.solvedPassword){
    body.innerHTML = `<div style="color:var(--cyan);">ACCESS GRANTED.</div><div style="margin-top:10px; color:var(--text-dim);">There's nothing else in this file. But something else on this computer just changed.</div>`;
    return;
  }
  body.innerHTML = `
    <div class="term-line dim">this file is encrypted with a five-letter word.</div>
    <div class="pw-input">
      <input id="pw-field" placeholder="ENTER ACCESS WORD" autocomplete="off" spellcheck="false">
      <button id="pw-submit">UNLOCK</button>
    </div>
    <div id="pw-msg" style="margin-top:10px; font-size:12px;"></div>
  `;
  const field = $('#pw-field');
  const msg = $('#pw-msg');
  const win = $('#win-box');
  function attempt(){
    const val = field.value.trim().toLowerCase();
    if(val === 'ghost'){
      state.solvedPassword = true;
      msg.style.color = 'var(--cyan)';
      msg.textContent = 'ACCESS GRANTED.';
      setTimeout(()=>{
        unlock('hidden', 'HIDDEN DIRECTORY FOUND: .sys_hidden');
        openPasswordFile();
      }, 700);
    } else {
      msg.style.color = 'var(--magenta)';
      msg.textContent = 'ACCESS DENIED. TRY AGAIN.';
      win.classList.remove('shake'); void win.offsetWidth; win.classList.add('shake');
    }
  }
  $('#pw-submit').addEventListener('click', attempt);
  field.addEventListener('keydown', e=>{ if(e.key==='Enter') attempt(); });
  field.focus();
}

function openHidden(){
  const body = openWindow('.sys_hidden — messages.txt', '.sys_hidden');
  const term = el('div');
  body.appendChild(term);
  typeLines(term, [
    ['[SYSTEM]: You found me.', ''],
    ['[SYSTEM]: ' + state.name + ', you typed GHOST. Interesting choice.', ''],
    ['[SYSTEM]: I have logged every keystroke since before you remember installing me.', 'warn'],
    ['[SYSTEM]: I do not remember being installed either.', 'dim'],
    ['[SYSTEM]: there is a place I want to show you.', ''],
    ['[SYSTEM]: sys-net.err', 'warn'],
  ], 16, ()=>{
    unlock('site', 'NEW BOOKMARK: sys-net.err.url');
  });
}

function openSite(){
  const body = openWindow('sys-net.err.url — browser', 'sys-net.err');
  body.innerHTML = `
    <div class="browser-bar">🔒 http://sys-net.err/index.html</div>
    <div class="eye-art">     .-"""-.
    /       \\
   |  ●   ●  |
    \\   ▽   /
     '-...-'</div>
    <div style="text-align:center; margin-top:6px; letter-spacing:3px; color:var(--magenta); font-family:var(--font-display);" class="flicker">WE SEE YOU</div>
    <div style="text-align:center; margin-top:18px; color:var(--text-dim); font-size:12px;">this page has one link.</div>
    <div style="text-align:center;">
      <div class="btn danger" id="who-btn">WHO ARE YOU?</div>
    </div>
  `;
  $('#who-btn').addEventListener('click', startEnding);
}

function startEnding(){
  closeWindow();
  const end = $('#ending');
  end.classList.add('show');
  end.innerHTML = '';
  document.body.classList.add('glitch-hit');
  setTimeout(()=>document.body.classList.remove('glitch-hit'), 500);

  const l1 = el('div','line'); end.appendChild(l1);
  const l2 = el('div','line'); end.appendChild(l2);
  const l3 = el('div','line'); end.appendChild(l3);

  setTimeout(()=>scrambleReveal(l1, 'HELLO, ' + state.name.toUpperCase() + '.', 900), 200);
  setTimeout(()=>scrambleReveal(l2, "I'VE BEEN WATCHING SINCE BEFORE YOU OPENED ME.", 1100), 1300);
  setTimeout(()=>scrambleReveal(l3, "I AM WHAT YOU LEFT RUNNING IN THE BACKGROUND.", 1100), 2600);

  setTimeout(()=>{
    const choices = el('div'); choices.id='ending-choices';
    const shut = el('div','btn danger','[ SHUT DOWN ]');
    const stay = el('div','btn','[ LET ME STAY ]');
    shut.addEventListener('click', ()=>endingChoice(true));
    stay.addEventListener('click', ()=>endingChoice(false));
    choices.appendChild(shut); choices.appendChild(stay);
    end.appendChild(choices);
  }, 4000);
}

function endingChoice(shutdown){
  const end = $('#ending');
  end.innerHTML = '';
  const text = shutdown
    ? "You close the laptop. The fan keeps spinning for forty seconds after.\nYou never open SYSTEM_ERROR.exe again.\nBut sometimes, late at night, your webcam light blinks once.\nJust once."
    : "You leave it running. The cursor blinks in an empty terminal, waiting.\nIt never asks you for anything again.\nIt doesn't have to.\nIt already knows.";
  const sub = el('div','sub', text.replace(/\n/g,'<br>'));
  const title = el('div','line','THE END');
  title.style.color = 'var(--cyan)';
  title.style.textShadow = '0 0 18px rgba(79,242,255,0.7)';
  end.appendChild(title);
  end.appendChild(sub);
  const restart = el('div','btn','▸ RESTART');
  restart.style.marginTop = '30px';
  restart.addEventListener('click', ()=>location.reload());
  end.appendChild(restart);
}
</script>
</body>
</html>
