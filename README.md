// Get the canvas element
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

// Define the game objects
const bubbles = [];
const shooter = {
  x: canvas.width / 2,
  y: canvas.height - 50,
  angle: 0,
  power: 10
};

// Draw the game objects
function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = 'rgba(255, 255, 255, 0.5)';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  // Draw bubbles
  bubbles.forEach((bubble) => {
    ctx.beginPath();
    ctx.arc(bubble.x, bubble.y, bubble.radius, 0, 2 * Math.PI);
    ctx.fillStyle = bubble.color;
    ctx.fill();
  });

  // Draw shooter
  ctx.beginPath();
  ctx.arc(shooter.x, shooter.y, 10, 0, 2 * Math.PI);
  ctx.fillStyle = 'black';
  ctx.fill();
}

// Update game state
function update() {
  // Update shooter position
  shooter.x += Math.cos(shooter.angle) * shooter.power;
  shooter.y += Math.sin(shooter.angle) * shooter.power;

  // Check for collisions
  bubbles.forEach((bubble) => {
    if (distance(shooter.x, shooter.y, bubble.x, bubble.y) < bubble.radius + 10) {
      // Remove bubble from game state
      bubbles.splice(bubbles.indexOf(bubble), 1);
      // Update score
      score++;
    }
  });
}

// Handle user input
canvas.addEventListener('click', (e) => {
  // Get mouse position
  const mouseX = e.clientX;
  const mouseY = e.clientY;

  // Calculate shooter angle
  shooter.angle = Math.atan2(mouseY - shooter.y, mouseX - shooter.x);
});

// Animation loop
setInterval(() => {
  update();
  draw();
}, 16); // 60fps
