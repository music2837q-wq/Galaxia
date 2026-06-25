<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Galaxia HTML Animada</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    html, body {
      width: 100%;
      height: 100%;
      overflow: hidden;
      background: radial-gradient(circle at center, #140022 0%, #090014 40%, #02030a 100%);
      font-family: Arial, sans-serif;
    }

    canvas {
      display: block;
      width: 100vw;
      height: 100vh;
    }

    .overlay {
      position: fixed;
      top: 18px;
      left: 18px;
      color: white;
      background: rgba(0,0,0,.28);
      border: 1px solid rgba(255,255,255,.15);
      padding: 12px 14px;
      border-radius: 14px;
      backdrop-filter: blur(6px);
      box-shadow: 0 8px 24px rgba(0,0,0,.35);
      z-index: 10;
      max-width: 320px;
    }

    .overlay h1 {
      font-size: 1rem;
      margin-bottom: 6px;
    }

    .overlay p {
      font-size: .92rem;
      line-height: 1.4;
      opacity: .92;
    }

    .badge {
      display: inline-block;
      margin-top: 8px;
      padding: 6px 10px;
      border-radius: 999px;
      background: linear-gradient(90deg, #7f5cff, #33d1ff);
      color: #fff;
      font-size: .8rem;
      font-weight: bold;
      letter-spacing: .2px;
    }
  </style>
</head>
<body>
  <div class="overlay">
    <h1>🌌 Galaxia HTML animada</h1>
    <p>Hecha con <strong>HTML + CSS + JavaScript</strong>. Puedes abrir este archivo en tu navegador y editar colores, cantidad de estrellas o velocidad de rotación.</p>
    <div class="badge">Lista para usar</div>
  </div>

  <canvas id="galaxy"></canvas>

  <script>
    const canvas = document.getElementById('galaxy');
    const ctx = canvas.getContext('2d');

    let w, h, cx, cy;
    function resize() {
      w = canvas.width = window.innerWidth * devicePixelRatio;
      h = canvas.height = window.innerHeight * devicePixelRatio;
      canvas.style.width = window.innerWidth + 'px';
      canvas.style.height = window.innerHeight + 'px';
      ctx.setTransform(1, 0, 0, 1, 0, 0);
      ctx.scale(devicePixelRatio, devicePixelRatio);
      cx = window.innerWidth / 2;
      cy = window.innerHeight / 2;
    }
    window.addEventListener('resize', resize);
    resize();

    const stars = [];
    const STAR_COUNT = 1800;
    const ARMS = 5;
    const MAX_RADIUS = Math.min(window.innerWidth, window.innerHeight) * 0.42;

    function rand(min, max) {
      return Math.random() * (max - min) + min;
    }

    for (let i = 0; i < STAR_COUNT; i++) {
      const arm = i % ARMS;
      const radius = Math.pow(Math.random(), 0.65) * MAX_RADIUS;
      const baseAngle = (arm / ARMS) * Math.PI * 2;
      const spiral = radius * 0.045;
      const angle = baseAngle + spiral + rand(-0.35, 0.35);

      stars.push({
        radius,
        angle,
        size: rand(0.4, 2.4),
        hue: rand(180, 320),
        alpha: rand(0.35, 1),
        speed: rand(0.0006, 0.0025),
        twinkle: rand(0.002, 0.02)
      });
    }

    const backgroundStars = Array.from({ length: 350 }, () => ({
      x: rand(0, window.innerWidth),
      y: rand(0, window.innerHeight),
      size: rand(0.4, 1.8),
      alpha: rand(0.2, 0.9),
      twinkle: rand(0.002, 0.015)
    }));

    let t = 0;

    function drawBackground() {
      const grad = ctx.createRadialGradient(cx, cy, 0, cx, cy, Math.max(window.innerWidth, window.innerHeight));
      grad.addColorStop(0, 'rgba(40, 0, 70, 0.18)');
      grad.addColorStop(0.4, 'rgba(8, 10, 30, 0.12)');
      grad.addColorStop(1, 'rgba(1, 2, 8, 1)');
      ctx.fillStyle = grad;
      ctx.fillRect(0, 0, window.innerWidth, window.innerHeight);

      for (const s of backgroundStars) {
        const flicker = 0.55 + Math.sin(t * s.twinkle * 100 + s.x) * 0.45;
        ctx.beginPath();
        ctx.fillStyle = `rgba(255,255,255,${s.alpha * flicker})`;
        ctx.arc(s.x, s.y, s.size, 0, Math.PI * 2);
        ctx.fill();
      }
    }

    function drawCore() {
      const core = ctx.createRadialGradient(cx, cy, 0, cx, cy, 120);
      core.addColorStop(0, 'rgba(255,255,255,1)');
      core.addColorStop(0.12, 'rgba(255,245,220,0.95)');
      core.addColorStop(0.35, 'rgba(255,180,120,0.35)');
      core.addColorStop(1, 'rgba(255,120,40,0)');
      ctx.fillStyle = core;
      ctx.beginPath();
      ctx.arc(cx, cy, 120, 0, Math.PI * 2);
      ctx.fill();
    }

    function drawGalaxy() {
      ctx.save();
      ctx.globalCompositeOperation = 'lighter';

      for (const s of stars) {
        s.angle += s.speed;
        const x = cx + Math.cos(s.angle) * s.radius;
        const y = cy + Math.sin(s.angle) * s.radius * 0.55;

        const flicker = 0.7 + Math.sin(t * 70 * s.twinkle + s.radius) * 0.3;
        const glow = ctx.createRadialGradient(x, y, 0, x, y, s.size * 6);
        glow.addColorStop(0, `hsla(${s.hue}, 100%, 88%, ${0.95 * s.alpha * flicker})`);
        glow.addColorStop(0.25, `hsla(${s.hue}, 100%, 70%, ${0.35 * s.alpha * flicker})`);
        glow.addColorStop(1, `hsla(${s.hue}, 100%, 50%, 0)`);

        ctx.fillStyle = glow;
        ctx.beginPath();
        ctx.arc(x, y, s.size * 6, 0, Math.PI * 2);
        ctx.fill();

        ctx.beginPath();
        ctx.fillStyle = `hsla(${s.hue}, 100%, 90%, ${s.alpha * flicker})`;
        ctx.arc(x, y, s.size, 0, Math.PI * 2);
        ctx.fill();
      }

      ctx.restore();
    }

    function animate() {
      t += 0.016;
      drawBackground();
      drawGalaxy();
      drawCore();
      requestAnimationFrame(animate);
    }

    animate();
  </script>
</body>
</html>
