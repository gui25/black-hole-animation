# Black Hole 3D Animation Script

Features:

Responsive design: Automatically adapts to the user's screen size.

Lightweight: No external libraries or dependencies required.

Smooth animation: High-quality gravitational pull effect using HTML5 Canvas.

Mouse-controlled center of attraction: The black hole's center of attraction follows the user's mouse cursor.

Usage:

Copy and paste the provided script into your Devtools console in any page. Enjoy the 3D black hole animation on your webpage, with particles being attracted to the mouse cursor!

Script:

```
(function() {
  const canvas = document.createElement('canvas');
  canvas.id = 'canvasBlackHole';
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  canvas.style.position = 'fixed';
  canvas.style.zIndex = 1000;
  canvas.style.top = '0';
  canvas.style.left = '0';

  document.body.innerHTML = '';

  document.body.appendChild(canvas);

  window.addEventListener('resize', function() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  });

  let mouseX = canvas.width / 2;
  let mouseY = canvas.height / 2;

  canvas.addEventListener('mousemove', function(event) {
    mouseX = event.clientX;
    mouseY = event.clientY;
  });

  const particleCount = 1000;
  const particles = [];

  for (let i = 0; i < particleCount; i++) {
    particles.push({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      angle: Math.random() * Math.PI * 2,
      speed: Math.random() * 0.5 + 0.5,
    });
  }

  function renderFrame() {
    const ctx = canvas.getContext('2d');
    ctx.fillStyle = '#000';
    ctx.fillRect(0, 0, ctx.canvas.width, ctx.canvas.height);

    for (const particle of particles) {
      const dx = mouseX - particle.x;
      const dy = mouseY - particle.y;
      const distance = Math.sqrt(dx * dx + dy * dy);
      const directionX = dx / distance;
      const directionY = dy / distance;

      particle.angle += 0.01;
      particle.x += particle.speed * Math.cos(particle.angle) + directionX;
      particle.y += particle.speed * Math.sin(particle.angle) + directionY;

      const lightIntensity = Math.max(0, 1 - distance / 200);

      if (lightIntensity > 0) {
        ctx.fillStyle = `rgba(255,255,255,${lightIntensity})`;
        ctx.fillRect(particle.x, particle.y, 2, 2);
      }
    }
  }

  let animationInterval;
  function toggleAnimation() {
    if (animationInterval === undefined) {
      animationInterval = setInterval(renderFrame, 10);
    } else {
      clearInterval(animationInterval);
      animationInterval = undefined;
    }
  }

  toggleAnimation();
})();
```
