# awayvomit.github.io
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Valentine</title>

<style>
:root{
  --bg1:#f6d365;
  --bg2:#fda085;
  --accent:#ff4d6d;
  --text:#333;
}

*{box-sizing:border-box}
html,body{
  margin:0;
  height:100%;
  font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,sans-serif;
  background:linear-gradient(135deg,var(--bg1),var(--bg2));
}

.app{
  height:100%;
  display:flex;
  align-items:center;
  justify-content:center;
  overflow:hidden;
}

/* ---------- SCREENS ---------- */

.screen{
  position:absolute;
  inset:0;
  display:flex;
  align-items:center;
  justify-content:center;
  transition:opacity 1s ease, transform 1s ease;
}

.hidden{
  opacity:0;
  pointer-events:none;
  transform:scale(.98);
}

/* ---------- ENVELOPE ---------- */

.envelope-wrap{
  perspective:1000px;
}

.envelope{
  width:260px;
  height:180px;
  position:relative;
  cursor:pointer;
  transform-style:preserve-3d;
}

/* visual envelope shell (clips) */
.envelope-shell{
  position:absolute;
  inset:0;
  background:#fff;
  border-radius:14px;
  box-shadow:0 18px 40px rgba(0,0,0,.18);
  overflow:hidden;
  z-index:3;
}

/* bottom fold */
.envelope-bottom{
  position:absolute;
  inset:0;
  background:#f2f2f2;
  clip-path:polygon(0 100%,50% 55%,100% 100%);
}

/* top flap */
.envelope-flap{
  position:absolute;
  inset:0;
  background:#fff;
  clip-path:polygon(0 0,100% 0,50% 55%);
  transform-origin:top center;
  transform:rotateX(0deg);
  transition:transform .7s cubic-bezier(.4,0,.2,1);
  backface-visibility:hidden;
  z-index:5;
}

.envelope.open .envelope-flap{
  transform:rotateX(-155deg);
}

/* ---------- LETTER (NOT CLIPPED) ---------- */

.letter{
  position:absolute;
  left:50%;
  bottom:260px;
  width:220px;
  padding:18px 16px 20px;
  background:
    linear-gradient(0deg,rgba(255,255,255,.96),rgba(255,255,255,.96)),
    repeating-linear-gradient(
      45deg,
      rgba(0,0,0,.015) 0px,
      rgba(0,0,0,.015) 1px,
      transparent 1px,
      transparent 4px
    );
  border-radius:14px;
  transform:translateX(-50%) translateY(120%);
  transition:transform 1.5s cubic-bezier(.4,0,.2,1) .35s;
  box-shadow:0 14px 30px rgba(0,0,0,.18);
  text-align:center;
  z-index:2;
}

.envelope.open .letter{
  transform:translateX(-50%) translateY(90px);
}

.letter h2{
  margin:0 0 12px;
  font-size:1.05rem;
  color:var(--text);
}

.letter p{
  margin:0 0 16px;
  font-size:.95rem;
  color:#555;
}

/* ---------- BUTTONS ---------- */

.actions{
  display:flex;
  gap:12px;
  justify-content:center;
}

button{
  border:none;
  padding:10px 18px;
  border-radius:999px;
  font-size:.9rem;
  cursor:pointer;
  transition:transform .2s ease;
}

.yes{
  background:var(--accent);
  color:#fff;
}

.yes:hover{
  transform:scale(1.10);
}

.no{
  background:#eee;
  color:#666;
  position:relative;
  
}

.no.dissolve {
  pointer-events: none;
  transition: opacity 1.5s ease;
  opacity: 0;

}

/* ---------- FINAL ---------- */

.final-card{
  background:#fff;
  padding:36px 30px;
  border-radius:24px;
  box-shadow:0 30px 60px rgba(0,0,0,.2);
  text-align:center;
}

canvas{
  position:fixed;
  inset:0;
  pointer-events:none;
}
</style>
</head>

<body>
<div class="app">

<!-- ENVELOPE SCREEN -->
<div class="screen envelope-screen">
  <div class="envelope-wrap">
    <div class="envelope" id="envelope">

      <!-- LETTER (sibling, not clipped) -->
      <div class="letter">
        <h2>URGENT</h2>
        <p>Will you be my valentine? 🌹   </p>
        <div class="actions">
          <button class="yes" id="yesBtn">Yes😌</button>
          <button class="no" id="noBtn">No🥀 </button>
        </div>
      </div>

      <!-- ENVELOPE VISUAL -->
      <div class="envelope-shell">
        <div class="envelope-flap"></div>
        <div class="envelope-bottom"></div>
      </div>

    </div>
  </div>
</div>

<!-- FINAL SCREEN -->
<div class="screen final hidden">
  <div class="final-card">
    <h1>Happy Valentines 😍</h1>
    <p>You're getting creampied 🥧  🥛 </p>
  </div>
</div>

</div>

<canvas id="confetti"></canvas>

<script>
const envelope=document.getElementById("envelope");
const noBtn=document.getElementById("noBtn");
const yesBtn=document.getElementById("yesBtn");
const finalScreen=document.querySelector(".final");

envelope.addEventListener("click",()=>{
  envelope.classList.add("open");
});

/* No button dodge */
noBtn.addEventListener("mouseenter",()=>{
  const x=(Math.random()-.5)*120;
  const y=(Math.random()-.5)*80;
  noBtn.style.transform=`translate(${x}px,${y}px)`;
});

noBtn.addEventListener("click", () => {
  noBtn.classList.add("dissolve");
});


/* Yes click */
yesBtn.addEventListener("click",()=>{
  finalScreen.classList.remove("hidden");
  startConfetti();
});

/* Confetti */
const canvas=document.getElementById("confetti");
const ctx=canvas.getContext("2d");
let confetti=[];

function resize(){
  canvas.width=innerWidth;
  canvas.height=innerHeight;
}
addEventListener("resize",resize);
resize();

function startConfetti(){
  confetti=Array.from({length:120},()=>({
    x:Math.random()*canvas.width,
    y:-20,
    r:Math.random()*6+4,
    c:`hsl(${Math.random()*360},80%,60%)`,
    v:Math.random()*3+2
  }));
  requestAnimationFrame(update);
}

function update(){
  ctx.clearRect(0,0,canvas.width,canvas.height);
  confetti.forEach(p=>{
    ctx.fillStyle=p.c;
    ctx.beginPath();
    ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
    ctx.fill();
    p.y+=p.v;
  });
  confetti=confetti.filter(p=>p.y<canvas.height+20);
  if(confetti.length) requestAnimationFrame(update);
}
</script>
</body>
</html>
