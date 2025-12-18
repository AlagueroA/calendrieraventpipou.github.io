---
layout: page
title: Avalanche de coeurs
permalink: /wheelrestaurant
---

<div style="
  max-width: 420px;
  margin: 40px auto;
  padding: 25px;
  border-radius: 20px;
  background: #ffffff;
  box-shadow: 0 15px 40px rgba(0,0,0,0.15);
  text-align: center;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
">

  <div style="font-size:24px; font-weight:700; margin-bottom:10px;">
    🎡 Decision Wheel
  </div>

  <canvas id="wheel" width="300" height="300"
          style="display:block; margin:0 auto;"></canvas>

  <button id="spinBtn"
          style="
            margin-top: 20px;
            padding: 12px 26px;
            font-size: 18px;
            border-radius: 999px;
            border: none;
            background: linear-gradient(135deg, #ff7a18, #ffb347);
            color: white;
            cursor: pointer;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
            transition: transform 0.2s, box-shadow 0.2s;
          "
          onmouseover="this.style.transform='translateY(-2px)'; this.style.boxShadow='0 14px 28px rgba(0,0,0,0.25)'"
          onmouseout="this.style.transform='translateY(0)'; this.style.boxShadow='0 10px 20px rgba(0,0,0,0.2)'"
          onclick="spinWheel()">
    Spin the wheel
  </button>

  <div id="result"
       style="margin-top:20px; font-size:22px; font-weight:600; color:#2a9d8f;">
  </div>

</div>

<script>
(() => {
  const options = [
    "Go for a walk 🚶",
    "Read a book 📖",
    "Watch a movie 🎬",
    "Cook something 🍳",
    "Take a nap 😴",
    "Learn something new 🧠"
  ];

  const canvas = document.getElementById("wheel");
  const ctx = canvas.getContext("2d");
  const radius = canvas.width / 2;
  let angle = 0;
  let spinning = false;

  function drawWheel() {
    const slice = (2 * Math.PI) / options.length;
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Wheel shadow
    ctx.save();
    ctx.shadowColor = "rgba(0,0,0,0.25)";
    ctx.shadowBlur = 15;
    ctx.beginPath();
    ctx.arc(radius, radius, radius - 5, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();

    options.forEach((opt, i) => {
      ctx.beginPath();
      ctx.moveTo(radius, radius);
      ctx.arc(radius, radius, radius - 10,
              angle + i * slice,
              angle + (i + 1) * slice);
      ctx.fillStyle = `hsl(${i * 360 / options.length}, 75%, 55%)`;
      ctx.fill();
      ctx.stroke();

      ctx.save();
      ctx.translate(radius, radius);
      ctx.rotate(angle + (i + 0.5) * slice);
      ctx.textAlign = "right";
      ctx.fillStyle = "#000";
      ctx.font = "16px sans-serif";
      ctx.fillText(opt, radius - 20, 5);
      ctx.restore();
    });

    // Center cap
    ctx.beginPath();
    ctx.arc(radius, radius, 10, 0, Math.PI * 2);
    ctx.fillStyle = "#333";
    ctx.fill();

    // Pointer
    ctx.beginPath();
    ctx.moveTo(radius, 8);
    ctx.lineTo(radius - 14, 36);
    ctx.lineTo(radius + 14, 36);
    ctx.closePath();
    ctx.fillStyle = "#e63946";
    ctx.fill();
  }

  window.spinWheel = function () {
    if (spinning) return;
    spinning = true;
    document.getElementById("result").textContent = "";

    const spinAngle = Math.random() * 2000 + 2000;
    const duration = 3000;
    const start = performance.now();

    function animate(time) {
      const progress = Math.min((time - start) / duration, 1);
      const easeOut = 1 - Math.pow(1 - progress, 3);
      angle = (spinAngle * easeOut) * Math.PI / 180;
      drawWheel();

      if (progress < 1) {
        requestAnimationFrame(animate);
      } else {
        spinning = false;
        const slice = (2 * Math.PI) / options.length;
        const index =
          options.length -
          Math.floor((angle % (2 * Math.PI)) / slice) - 1;

        document.getElementById("result").textContent =
          "✅ Result: " + options[index];
      }
    }

    requestAnimationFrame(animate);
  };

  drawWheel();
})();
</script>
