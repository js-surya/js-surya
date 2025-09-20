<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Synt4x404 - GIS & Data Engineer</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f8f9fa;
      margin: 20px;
      line-height: 1.6;
    }
    h1, h2, h3 {
      color: #2c3e50;
    }
    blockquote {
      font-style: italic;
      background: #eef2f7;
      padding: 10px 15px;
      border-left: 5px solid #3b82f6;
      margin: 15px 0;
    }
    #game {
      margin: 20px 0;
      padding: 20px;
      background: #ffffff;
      border: 2px solid #ddd;
      border-radius: 10px;
      text-align: center;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    }
    canvas {
      border: 2px solid #2c3e50;
      border-radius: 8px;
      background: #fff;
    }
  </style>
</head>
<body>

<h1>Hi, I'm Synt4x404 👋</h1>

<h2>🌍 GIS Enthusiast & Data Engineer</h2>
<p>
I specialize in <strong>geospatial data analysis, visualization, and geoprocessing</strong>.  
Passionate about leveraging GIS technologies to solve real-world spatial problems and create impactful maps and applications.
</p>

<hr>

<h2>🛠️ GIS Software & Tools</h2>
<ul>
  <li><strong>QGIS</strong> – Open-source GIS software</li>
  <li><strong>ArcGIS</strong> – ESRI geospatial suite</li>
  <li><strong>PostGIS</strong> – Spatial database extender for PostgreSQL</li>
  <li><strong>GDAL</strong> – Geospatial Data Abstraction Library</li>
  <li><strong>Leaflet</strong> – JS library for interactive maps</li>
</ul>

<hr>

<h2>💻 Programming Languages</h2>
<ul>
  <li><strong>Python</strong> – GeoPandas, Rasterio, Shapely, Machine Learning</li>
  <li><strong>R</strong> – sf, raster, tmap for spatial statistics & visualization</li>
  <li><strong>Shell scripting</strong> – Automation & data pipelines</li>
  <li><strong>SQL</strong> – Spatial queries & database management</li>
</ul>

<hr>

<h2>🌐 Networking & Server Skills</h2>
<ul>
  <li><strong>Tailscale</strong> – Zero-config VPN & secure networking</li>
  <li><strong>Linux server administration</strong> – Deployment, automation, monitoring</li>
  <li><strong>Docker & containerization</strong> – Building and running scalable applications</li>
  <li><strong>NGINX & Apache</strong> – Web server configuration & optimization</li>
  <li><strong>Cloud (AWS/GCP)</strong> – Hosting geospatial apps and data pipelines</li>
</ul>

<hr>

<h2>🎮 Game of the Day</h2>
<div id="game">
  <h3>🗺️ Coordinate Catch</h3>
  <p>Move the square with arrow keys ⬆️⬇️⬅️➡️ and catch the circle!</p>
  <canvas id="canvas" width="400" height="400"></canvas>
</div>

<script>
  const canvas = document.getElementById("canvas");
  const ctx = canvas.getContext("2d");

  let player = {x: 180, y: 180, size: 20, color: "#3b82f6"};
  let target = {x: 100, y: 100, size: 15, color: "#e63946"};
  let score = 0;

  function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Player
    ctx.fillStyle = player.color;
    ctx.fillRect(player.x, player.y, player.size, player.size);

    // Target
    ctx.fillStyle = target.color;
    ctx.beginPath();
    ctx.arc(target.x, target.y, target.size, 0, Math.PI * 2);
    ctx.fill();

    // Score
    ctx.fillStyle = "#2c3e50";
    ctx.font = "16px Arial";
    ctx.fillText("Score: " + score, 10, 20);
  }

  function randomPos(max) {
    return Math.floor(Math.random() * (max - 30)) + 15;
  }

  function movePlayer(e) {
    if (e.key === "ArrowUp" && player.y > 0) player.y -= 10;
    if (e.key === "ArrowDown" && player.y < canvas.height - player.size) player.y += 10;
    if (e.key === "ArrowLeft" && player.x > 0) player.x -= 10;
    if (e.key === "ArrowRight" && player.x < canvas.width - player.size) player.x += 10;

    // Collision check
    let dx = player.x - target.x;
    let dy = player.y - target.y;
    let distance = Math.sqrt(dx * dx + dy * dy);
    if (distance < player.size) {
      score++;
      target.x = randomPos(canvas.width);
      target.y = randomPos(canvas.height);
    }

    draw();
  }

  document.addEventListener("keydown", movePlayer);
  draw();
</script>

<hr>

<h2>💡 Quote of the Day</h2>
<blockquote id="quote">Loading today's quote...</blockquote>
<script>
  async function loadDailyQuote() {
    try {
      const res = await fetch("https://zenquotes.io/api/today");
      const data = await res.json();
      document.getElementById("quote").textContent = `"${data[0].q}" — ${data[0].a}`;
    } catch (err) {
      document.getElementById("quote").textContent = "Failed to load today's quote.";
    }
  }
  loadDailyQuote();
</script>

<hr>

<p>✨ Feel free to explore my projects and reach out to collaborate or chat about <strong>GIS, networking, coding, or tech</strong>!</p>

</body>
</html>
