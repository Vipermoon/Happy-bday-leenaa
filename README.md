<!DOCTYPE html> 
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Happy Birthday Leena!</title>
  <link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display&display=swap" rel="stylesheet">
  <style>
    html, body {
      margin: 0;
      padding: 0;
      background-color: black;
      overflow: hidden;
      font-family: 'DM Serif Display', serif;
    }

    canvas {
      display: block;
      position: absolute;
      top: 0;
      left: 0;
      z-index: 0;
    }

    #message {
      position: absolute;
      top: 40%;
      left: 50%;
      transform: translate(-50%, -50%);
      color: #ffffff;
      font-size: 3em;
      text-align: center;
      display: none;
      z-index: 4;
      text-shadow:
        0 0 10px #00f7ff,
        0 0 20px #00f7ff,
        0 0 40px #ff00ff,
        0 0 60px #ff00ff,
        0 0 80px #ff0080;
      animation: glow 2s ease-in-out infinite alternate;
    }

    @keyframes glow {
      from {
        text-shadow:
          0 0 10px #00f7ff,
          0 0 20px #00f7ff,
          0 0 40px #ff00ff;
      }
      to {
        text-shadow:
          0 0 20px #00f7ff,
          0 0 40px #00f7ff,
          0 0 60px #ff00ff;
      }
    }

    #present-container {
      position: absolute;
      top: 60%;
      left: 50%;
      transform: translate(-50%, -50%);
      text-align: center;
      z-index: 3;
      display: none;
      flex-direction: column;
      align-items: center;
      cursor: pointer;
    }

    #open-text {
      color: white;
      font-size: 1.8em;
      margin-bottom: 10px;
      font-family: 'DM Serif Display', serif;
      animation: glow 1.5s ease-in-out infinite alternate;
      user-select: none;
    }

    #arrow {
      font-size: 2em;
      color: white;
      animation: glow 1.5s ease-in-out infinite alternate;
      user-select: none;
    }

    #present {
      width: 150px;
      height: 150px;
      background: linear-gradient(45deg, #ff69b4, #ff1493);
      border: 4px solid white;
      border-radius: 20px;
      position: relative;
      box-shadow: 0 0 25px #ff99cc;
      transition: transform 0.3s ease;
    }

    #present:hover {
      transform: scale(1.05);
    }

    /* Ribbon */
    #present::before,
    #present::after {
      content: '';
      position: absolute;
      background: white;
    }

    #present::before {
      width: 100%;
      height: 15px;
      top: 50%;
      left: 0;
      transform: translateY(-50%);
    }

    #present::after {
      height: 100%;
      width: 15px;
      left: 50%;
      top: 0;
      transform: translateX(-50%);
    }

    /* Actual proper hearts */

  .heart {
    position: absolute;
    width: 40px;
    height: 36px;
    background: lightpink;
    transform: rotate(-45deg);
    animation: float 6s ease-in-out infinite;
    top: 0;
    left: 0;
    z-index: 2;
  }

  .heart::before,
  .heart::after {
    content: "";
    position: absolute;
    width: 40px;
    height: 36px;
    background: lightpink;
    border-radius: 50%;
    top: 0;
    left: 0;
  }

  .heart::before {
    top: -20px;
    left: 0;
  }

  .heart::after {
    top: 0;
    left: 20px;
  }
  
</style>

    @keyframes float {
      0%   { transform: translateY(0) rotate(-45deg); }
      50%  { transform: translateY(-20px) rotate(-45deg); }
      100% { transform: translateY(0) rotate(-45deg); }
    }
  </style>
</head>
<body>
  <canvas id="fireworks"></canvas>

  <div id="message">💖💖 Happy Birthday My Beautiful Leena 🎂💐</div>

  <div id="present-container">
    <div id="open-text">🎁 Open Me</div>
    <div id="arrow">⬇️</div>
    <div id="present"></div>
  </div>

  <script>
    const canvas = document.getElementById('fireworks');
    const ctx = canvas.getContext('2d');
    let particles = [];
    let width = window.innerWidth;
    let height = window.innerHeight;
    canvas.width = width;
    canvas.height = height;

    window.addEventListener('resize', () => {
      width = window.innerWidth;
      height = window.innerHeight;
      canvas.width = width;
      canvas.height = height;
    });

    function random(min, max) {
      return Math.random() * (max - min) + min;
    }

    function createFirework(x, y) {
      const colors = ['#ff2d95', '#ffd700', '#00ffff', '#ff4500', '#7fff00'];
      const color = colors[Math.floor(Math.random() * colors.length)];
      for (let i = 0; i < 150; i++) {
        particles.push({
          x: x,
          y: y,
          radius: 2,
          dx: random(-5, 5),
          dy: random(-5, 5),
          life: 100,
          color: color
        });
      }
    }

    function updateParticles() {
      ctx.fillStyle = "rgba(0, 0, 0, 0.2)";
      ctx.fillRect(0, 0, width, height);
      particles = particles.filter(p => p.life > 0);
      for (let p of particles) {
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
        ctx.fillStyle = p.color;
        ctx.fill();
        p.x += p.dx;
        p.y += p.dy;
        p.life--;
      }
    }

    function animateFireworks() {
      updateParticles();
      requestAnimationFrame(animateFireworks);
    }

    function startFireworkShow() {
      let count = 0;
      const launchInterval = setInterval(() => {
        const x = random(100, width - 100);
        const y = random(100, height / 2);
        createFirework(x, y);
        count++;
        if (count > 40) clearInterval(launchInterval);
      }, 100);
    }

    const heartCount = 10;
    let caught = 0;

    function spawnHearts() {
      for (let i = 0; i < heartCount; i++) {
        const heart = document.createElement('div');
        heart.classList.add('heart');
        heart.style.left = Math.random() * (window.innerWidth - 100) + 'px';
        heart.style.top = Math.random() * (window.innerHeight - 100) + 'px';
        document.body.appendChild(heart);

        heart.addEventListener('click', () => {
          heart.remove();
          caught++;
          if (caught === heartCount) {
            showPresent();
          }
        });
      }
    }

    const presentContainer = document.getElementById('present-container');
    const present = document.getElementById('present');
    const message = document.getElementById('message');

    function showPresent() {
      presentContainer.style.display = 'flex';
    }

    present.addEventListener('click', () => {
      presentContainer.style.display = 'none';
      message.style.display = 'block';
      startFireworkShow();
      animateFireworks();
    });

    spawnHearts();
  </script>
</body>
</html>
# Happy-bday-leenaa
