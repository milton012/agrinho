let player;
let fieldItems = [];
let cityItems = [];
let celebrationArea;
let score = 0;

function setup() {
  createCanvas(600, 400);
  player = new Player();
  celebrationArea = new CelebrationArea();
  // Gerar itens do campo
  for (let i = 0; i < 5; i++) {
    fieldItems.push(new FieldItem(random(50, 550), random(50, 350)));
  }
  // Gerar itens da cidade
  for (let i = 0; i < 5; i++) {
    cityItems.push(new CityItem(random(50, 550), random(50, 350)));
  }
}

function draw() {
  background(220);
  
  // Desenhar área de celebração
  celebrationArea.display();
  
  // Atualizar e desenhar itens do campo
  for (let item of fieldItems) {
    item.display();
  }
  
  // Atualizar e desenhar itens da cidade
  for (let item of cityItems) {
    item.display();
  }
  
  // Desenhar o jogador
  player.update();
  player.display();
  
  // Checar colisões e atualizar a pontuação
  for (let i = fieldItems.length - 1; i >= 0; i--) {
    if (player.collect(fieldItems[i])) {
      fieldItems.splice(i, 1);
      score += 10;
    }
  }
  
  for (let i = cityItems.length - 1; i >= 0; i--) {
    if (player.collect(cityItems[i])) {
      cityItems.splice(i, 1);
      score += 10;
    }
  }
  
  // Mostrar pontuação
  fill(0);
  textSize(20);
  text("Pontuação: " + score, 10, 30);
}

class Player {
  constructor() {
    this.x = width / 2;
    this.y = height - 30;
    this.size = 20;
    this.speed = 5;
  }

  update() {
    if (keyIsDown(LEFT_ARROW)) {
      this.x -= this.speed;
    }
    if (keyIsDown(RIGHT_ARROW)) {
      this.x += this.speed;
    }
    if (keyIsDown(UP_ARROW)) {
      this.y -= this.speed;
    }
    if (keyIsDown(DOWN_ARROW)) {
      this.y += this.speed;
    }
  }

  display() {
    fill(255, 0, 0);
    ellipse(this.x, this.y, this.size, this.size);
  }

  collect(item) {
    let d = dist(this.x, this.y, item.x, item.y);
    if (d < this.size / 2 + item.size / 2) {
      return true;
    }
    return false;
  }
}

class FieldItem {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.size = 15;
    this.color = color(34, 139, 34); // Cor do campo
  }

  display() {
    fill(this.color);
    ellipse(this.x, this.y, this.size, this.size);
  }
}

class CityItem {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.size = 15;
    this.color = color(100, 100, 255); // Cor da cidade
  }

  display() {
    fill(this.color);
    rect(this.x, this.y, this.size, this.size);
  }
}

class CelebrationArea {
  constructor() {
    this.x = width / 2 - 50;
    this.y = height / 2 - 50;
    this.width = 100;
    this.height = 100;
  }

  display() {
    fill(255, 255, 0, 150);
    rect(this.x, this.y, this.width, this.height);
  }
}
