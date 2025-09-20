Hi, I’m Synt4x404 👋

🌍 GIS Enthusiast & Data Engineer

I specialize in geospatial data analysis, visualization, and geoprocessing. Passionate about leveraging GIS technologies to solve real-world spatial problems and create impactful maps and applications.

⸻

🛠️ GIS Software & Tools
	•	

 QGIS – Open-source GIS software

	•	

 ArcGIS – ESRI geospatial suite

	•	

 PostGIS – Spatial database extender for PostgreSQL

	•	

 GDAL – Geospatial Data Abstraction Library

	•	

 Leaflet – JS library for interactive maps

⸻

💻 Programming Languages
	•	

 Python – GeoPandas, Rasterio, Shapely, Machine Learning

	•	

 R – sf, raster, tmap for spatial statistics & visualization

	•	Shell scripting – Automation & data pipelines
	•	SQL – Spatial queries & database management

⸻

🌐 Networking & Server Skills
	•	

 Tailscale – Zero-config VPN & secure networking

	•	Linux server administration – Deployment, automation, monitoring
	•	Docker & containerization – Building and running scalable applications
	•	NGINX & Apache – Web server configuration & optimization
	•	Cloud (AWS/GCP) – Hosting geospatial apps and data pipelines

⸻

🎮 Fun JS Game — Coordinate Catch (included in this Markdown)

Play a quick JavaScript game that tests your ability to recognise coordinates. Save the code below as coordinate-catch.html and open it in your browser. Use the Left / Right arrow keys or A / D to move the catcher. Catch the falling coordinate that matches the target city (displayed at the top).

How to play
	•	A city name is shown at the top (the target).
	•	Coordinates fall from the top — catch the correct one to score points.
	•	Wrong catches cost you a life. You have 3 lives.
	•	The game speeds up over time.

HTML + JavaScript (save as coordinate-catch.html)

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Coordinate Catch — Synt4x404</title>
  <style>
    body{font-family:system-ui,Segoe UI,Roboto,Arial;margin:0;display:flex;align-items:center;justify-content:center;height:100vh;background:linear-gradient(180deg,#0f172a,#021027);color:#e6f0ff}
    .game{width:760px;max-width:95%;background:rgba(255,255,255,0.04);padding:16px;border-radius:12px;box-shadow:0 8px 30px rgba(2,6,23,0.6)}
    header{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px}
    canvas{display:block;background:#061325;border-radius:8px;width:100%;height:420px}
    .info{display:flex;gap:12px;align-items:center}
    .pill{background:rgba(255,255,255,0.05);padding:6px 10px;border-radius:999px}
    .small{font-size:0.9rem;opacity:0.9}
    .center{display:flex;justify-content:center;margin-top:10px}
    button{padding:8px 12px;border-radius:8px;border:0;background:#0ea5a4;color:white;cursor:pointer}
    .footer{display:flex;justify-content:space-between;margin-top:8px;font-size:0.9rem}
  </style>
</head>
<body>
<div class="game">
  <header>
    <div>
      <h2 style="margin:0">Coordinate Catch</h2>
      <div class="small">Catch coordinates for the target city. Arrow keys / A D to move.</div>
    </div>
    <div class="info">
      <div class="pill">Target: <strong id="targetCity">Paris</strong></div>
      <div class="pill small">Score: <span id="score">0</span></div>
      <div class="pill small">Lives: <span id="lives">3</span></div>
    </div>
  </header>
  <canvas id="c" width="760" height="420"></canvas>
  <div class="center"><button id="startBtn">Start / Restart</button></div>
  <div class="footer"><div class="small">Made by Synt4x404 — GIS & JS fun</div><div class="small">Difficulty increases over time</div></div>

  <h3>💡 Quote of the Day</h3>
  <blockquote id="quote">Loading quote...</blockquote>
</div>

<script>
// Coordinate Catch Game
(() => {
  const cities = [
    {name:'Paris, France', coord:[48.8566,2.3522]},
    {name:'New York, USA', coord:[40.7128,-74.0060]},
    {name:'Tokyo, Japan', coord:[35.6895,139.6917]},
    {name:'Sydney, Australia', coord:[-33.8688,151.2093]},
    {name:'Cairo, Egypt', coord:[30.0444,31.2357]},
    {name:'Rio de Janeiro, Brazil', coord:[-22.9068,-43.1729]},
    {name:'Moscow, Russia', coord:[55.7558,37.6173]},
    {name:'Cape Town, South Africa', coord:[-33.9249,18.4241]},
    {name:'Berlin, Germany', coord:[52.5200,13.4050]},
    {name:'Toronto, Canada', coord:[43.6532,-79.3832]}
  ];

  const canvas = document.getElementById('c');
  const ctx = canvas.getContext('2d');
  const W = canvas.width, H = canvas.height;

  let catcher = {x: W/2 - 40, y: H - 40, w:80, h:14, speed:6};
  let falling = [];
  let score = 0, lives = 3;
  let target = null;
  let running = false;
  let spawnInterval = 1400; // ms
  let lastSpawn = 0;
  let lastTime = 0;
  let speedMultiplier = 1;

  const targetCityEl = document.getElementById('targetCity');
  const scoreEl = document.getElementById('score');
  const livesEl = document.getElementById('lives');
  const startBtn = document.getElementById('startBtn');

  function pickTarget(){
    target = cities[Math.floor(Math.random()*cities.length)];
    targetCityEl.textContent = target.name;
  }

  function spawn(){
    const isCorrect = Math.random() < 0.33;
    let data;
    if(isCorrect){ data = {text: `${target.coord[0].toFixed(4)}, ${target.coord[1].toFixed(4)}`, correct:true}; }
    else{
      const c = cities[Math.floor(Math.random()*cities.length)];
      if(c.name === target.name){
        data = {text: `${(c.coord[0]+(Math.random()-0.5)*5).toFixed(4)}, ${(c.coord[1]+(Math.random()-0.5)*5).toFixed(4)}`, correct:false};
      } else data = {text: `${c.coord[0].toFixed(4)}, ${c.coord[1].toFixed(4)}`, correct:false};
    }
    falling.push({x: Math.random()*(W-140)+70, y:-20, vy:1.2*speedMultiplier, text:data.text, correct:data.correct});
  }

  function reset(){
    falling = []; score = 0; lives = 3; spawnInterval = 1400; speedMultiplier = 1; pickTarget(); updateUI();
  }

  function updateUI(){ scoreEl.textContent = score; livesEl.textContent = lives; }

  function step(ts){
    if(!lastTime) lastTime = ts; const dt = ts - lastTime; lastTime = ts;
    if(!running) return;

    ctx.clearRect(0,0,W,H);
    lastSpawn += dt;
    if(lastSpawn > spawnInterval){ spawn(); lastSpawn = 0; spawnInterval = Math.max(550, spawnInterval - 8); speedMultiplier += 0.007; }

    for(let i=falling.length-1;i>=0;i--){
      const f = falling[i]; f.y += f.vy * (dt/16);
      ctx.beginPath(); ctx.fillStyle = 'rgba(14,165,164,0.12)'; ctx.strokeStyle = 'rgba(14,165,164,0.6)'; ctx.lineWidth=1.2; ctx.roundRect ? ctx.roundRect(f.x-60,f.y-18,120,36,8) : null; ctx.fillRect(f.x-60,f.y-18,120,36); ctx.strokeRect(f.x-60,f.y-18,120,36);
      ctx.fillStyle = '#e6f0ff'; ctx.font = '14px system-ui'; ctx.textAlign='center'; ctx.fillText(f.text, f.x, f.y+6);
      if(f.y > H-60){
        if(f.correct){ lives--; updateUI(); if(lives<=0){ endGame(); return; } }
        falling.splice(i,1);
      }
    }

    ctx.fillStyle = '#10b981'; ctx.fillRect(catcher.x, catcher.y, catcher.w, catcher.h);
    ctx.strokeStyle = '#064e3b'; ctx.strokeRect(catcher.x, catcher.y, catcher.w, catcher.h);

    requestAnimationFrame(step);
  }

  function endGame(){ running = false; alert(`Game over! Score: ${score}`); }

  const keys = {};
  window.addEventListener('keydown', e => { keys[e.key.toLowerCase()] = true; if(['arrowleft','arrowright','a','d'].includes(e.key.toLowerCase())) e.preventDefault(); });
  window.addEventListener('keyup', e => { keys[e.key.toLowerCase()] = false; });

  function inputLoop(){
    if(running){
      if(keys['arrowleft'] || keys['a']) catcher.x -= catcher.speed;
      if(keys['arrowright'] || keys['d']) catcher.x += catcher.speed;
      catcher.x = Math.max(8, Math.min(W-catcher.w-8, catcher.x));
      for(let i=falling.length-1;i>=0;i--){
        const f = falling[i];
        if(f.y + 18 >= catcher.y && f.y <= catcher.y + catcher.h){
          if(f.x > catcher.x && f.x < catcher.x + catcher.w){
            if(f.correct){ score += 10; }
            else{ lives--; }
            falling.splice(i,1); updateUI(); if(lives<=0){ endGame(); return; }
          }
        }
      }
    }
    setTimeout(inputLoop, 16);
  }

  if(!CanvasRenderingContext2D.prototype.roundRect){ CanvasRenderingContext2D.prototype.roundRect = function(x,y,w,h,r){ if(typeof r==='undefined') r=5; this.beginPath(); this.moveTo(x+r,y); this.arcTo(x+w,y,x+w,y+h,r); this.arcTo(x+w,y+h,x,y+h,r); this.arcTo(x,y+h,x,y,r); this.arcTo(x,y,x+w,y,r); this.closePath(); }}

  canvas.addEventListener('click', e => {
    if(!running) return;
    const rect = canvas.getBoundingClientRect(); const mx = e.clientX - rect.left; const my = e.clientY - rect.top;
    for(let i=falling.length-1;i>=0;i--){ const f=falling[i]; if(mx >= f.x-60 && mx <= f.x+60 && my >= f.y-18 && my <= f.y+18){ if(f.correct) score+=10; else lives--; falling.splice(i,1); updateUI(); if(lives<=0){ endGame(); return; } break; }}
  });

  startBtn.addEventListener('click', ()=>{
    reset(); running=true; lastTime=0; lastSpawn=0; requestAnimationFrame(step);
  });

  pickTarget(); updateUI(); inputLoop();
})();

// Daily Quote Fetcher
(async function loadDailyQuote(){
  try {
    const res = await fetch("https://zenquotes.io/api/today");
    const data = await res.json();
    document.getElementById("quote").textContent = `"${data[0].q}" — ${data[0].a}`;
  } catch (err) {
    document.getElementById("quote").textContent = "Failed to load today's quote.";
  }
})();
</script>
</body>
</html>


⸻

✨ Feel free to explore my projects and reach out to collaborate or chat about GIS, networking, coding, or tech!
