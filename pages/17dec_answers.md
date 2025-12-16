---
layout: page
title: Avalanche de coeurs
permalink: /17decanswers
---

<style>
  .heart {
    position: fixed;
    top: -10px;
    font-size: 20px;
    color: #ff4d6d;
    animation-name: fall, sway;
    animation-timing-function: linear, ease-in-out;
    pointer-events: none;
    z-index: 9999;
  }

  @keyframes fall {
    to {
      transform: translateY(110vh);
    }
  }

  @keyframes sway {
    0%   { margin-left: 0px; }
    50%  { margin-left: 30px; }
    100% { margin-left: 0px; }
  }
</style>

<script>
  function createHeart() {
    const heart = document.createElement("div");
    heart.className = "heart";
    heart.innerHTML = "❤️";

    heart.style.left = Math.random() * 100 + "vw";
    heart.style.fontSize = (12 + Math.random() * 24) + "px";
    heart.style.animationDuration =
      (3 + Math.random() * 4) + "s, " +
      (2 + Math.random() * 3) + "s";

    document.body.appendChild(heart);

    setTimeout(() => {
      heart.remove();
    }, 8000);
  }

  setInterval(createHeart, 50);
</script>
