let ball;
let leftPaddle, rightPaddle;
let ballSpeed = 5;
let paddleSpeed = 10;

function setup() {
  createCanvas(800, 400);
  ball = new Ball();
  leftPaddle = new Paddle(true);
  rightPaddle = new Paddle(false);
}

function draw() {
  background(0);
  
  ball.update();
  ball.display();
  
  leftPaddle.update();
  leftPaddle.display();
  
  rightPaddle.update();
  rightPaddle.display();
  
  checkCollisions();
}

function checkCollisions() {
  // Ball collision with paddles
  if (ball.x - ball.radius < leftPaddle.x + leftPaddle.width &&
      ball.y > leftPaddle.y &&
      ball.y < leftPaddle.y + leftPaddle.height) {
    ball.xSpeed *= -1;
  }
  
  if (ball.x + ball.radius > rightPaddle.x &&
      ball.y > rightPaddle.y &&
      ball.y < rightPaddle.y + rightPaddle.height) {
    ball.xSpeed *= -1;
  }

  // Ball out of bounds
  if (ball.x - ball.radius < 0 || ball.x + ball.radius > width) {
    ball.reset();
  }
}

class Ball {
  constructor() {
    this.reset();
  }
  
  reset() {
    this.x = width / 2;
    this.y = height / 2;
    this.radius = 10;
    this.xSpeed = random([ballSpeed, -ballSpeed]);
    this.ySpeed = random([ballSpeed, -ballSpeed]);
  }
  
  update() {
    this.x += this.xSpeed;
    this.y += this.ySpeed;
    
    if (this.y - this.radius < 0 || this.y + this.radius > height) {
      this.ySpeed *= -1;
    }
  }
  
  display() {
    fill(255);
    ellipse(this.x, this.y, this.radius * 2);
  }
}

class Paddle {
  constructor(isLeft) {
    this.isLeft = isLeft;
    this.width = 20;
    this.height = 100;
    this.x = isLeft ? 0 : width - this.width;
    this.y = height / 2 - this.height / 2;
  }
  
  update() {
    if (this.isLeft) {
      if (keyIsDown(87)) { // W key
        this.y -= paddleSpeed;
      }
      if (keyIsDown(83)) { // S key
        this.y += paddleSpeed;
      }
    } else {
      if (keyIsDown(UP_ARROW)) {
        this.y -= paddleSpeed;
      }
      if (keyIsDown(DOWN_ARROW)) {
        this.y += paddleSpeed;
      }
    }
    
    this.y = constrain(this.y, 0, height - this.height);
  }
  
  display() {
    fill(255);
    rect(this.x, this.y, this.width, this.height);
  }
}

