const colors = {
  backgroundTop: '#cce7ff',
  backgroundBottom: '#fefeff',
  textDark: '#111827',
  textGray: '#6b7280',
  boothFill: '#fef3c7',
  boothShadow: 'rgba(245, 158, 11, 0.25)',
  boothRoof: '#d97706',
  cityBuildingFill: '#e0e7ff',
  cityBuildingShadow: 'rgba(99, 102, 241, 0.15)',
  cityBuildingRoof: '#4338ca',
  cityWindow: '#bfdbfe',
  cityWindowFrame: '#c7d2fe',
  cityDoor: '#3730a3',
  flagPole: '#9ca3af',
  flagBlue: '#6366f1',
  flagWhite: '#f9fafb',
  stringColor: '#d1d5db',
  characterSkin: '#fde68a',
  characterClothes: '#4f46e5',
  characterHair: '#374151',
  groundCampoStart: '#fef6d8',
  groundCampoEnd: '#fde68a',
  groundCityStart: '#f2f5ff',
  groundCityEnd: '#dbe5ff',
  dividingLine: '#e5e7eb',
  street: '#444c55',
  streetStripe: '#fefefe',
  carBody1: '#374151',
  carWindow1: '#bfdbfe',
  carBody2: '#4f46e5',
  carWindow2: '#a5b4fc',
  benchWood: '#a9745a',
  benchShadow: 'rgba(169, 116, 90, 0.3)',
  benchLeg: '#6b4c3b',
};

let flags = [];
let lights = [];
let characters = [];
let cars = [];

function setup() {
  createCanvas(1200, 700);
  angleMode(DEGREES);
  textFont('system-ui, sans-serif');
  textAlign(CENTER, CENTER);
  noStroke();

  const flagCount = 20;
  for (let i = 0; i < flagCount; i++) {
    flags.push({
      x: map(i, 0, flagCount - 1, 100, width - 100),
      y: 140,
      phase: random(360),
      size: 24 + random(-4, 4),
    });
  }

  const lightCount = 60;
  for (let i = 0; i < lightCount; i++) {
    lights.push({
      x: random(width * 0.55 + 50, width - 80),
      y: random(height * 0.65 - 80, height * 0.65 + 40),
      on: random() > 0.5,
      timer: random(0, 60),
    });
  }

  // Characters: 2 near booths, 2 near bottom campo
  const boothWidth = 160;
  const boothSpacing = 70;
  const boothBaseX = 80;
  const baseY = height * 0.6 + 10;
  characters = [
    // Two characters near booths
    {
      x: boothBaseX + boothWidth * 0.3,
      yBase: baseY + 2,
      direction: 1,
      speed: 1.2,
      boothIndex: 0,
      boothX: boothBaseX,
      boothWidth: boothWidth,
    },
    {
      x: boothBaseX + (boothWidth + boothSpacing) + boothWidth * 0.6,
      yBase: baseY - 3,
      direction: -1,
      speed: 1,
      boothIndex: 1,
      boothX: boothBaseX + (boothWidth + boothSpacing),
      boothWidth: boothWidth,
    },
    // Two characters near bottom campo area
    {
      x: 180,
      yBase: height * 0.9,
      direction: 1,
      speed: 0.7,
      boothIndex: -1,
      boothX: null,
      boothWidth: 0,
    },
    {
      x: 240,
      yBase: height * 0.9 + 5,
      direction: -1,
      speed: 0.6,
      boothIndex: -1,
      boothX: null,
      boothWidth: 0,
    },
  ];

  // Cars on street
  cars = [
    {
      x: width * 0.55 + 10,
      y: height * 0.6 + height * 0.4 * 0.30,
      width: 90,
      height: 35,
      direction: 1,
      speed: 1.8,
      colorBody: colors.carBody1,
      colorWindow: colors.carWindow1,
    },
    {
      x: width - 100,
      y: height * 0.6 + height * 0.4 * 0.60,
      width: 100,
      height: 40,
      direction: -1,
      speed: 2.2,
      colorBody: colors.carBody2,
      colorWindow: colors.carWindow2,
    }
  ];

  noLoop();
}

function draw() {
  drawSkyGradient();

  drawHeader();
  drawGroundImproved();
  drawCampoBooths();
  drawCityBuildings();
  drawStreet();
  drawCars();
  drawBenchWithPeople();
  drawStringWithFlags();
  drawAnimatedLights();
  updateCharacters();
  updateCars();
}

function drawSkyGradient() {
  noFill();
  for (let y = 0; y <= height * 0.6; y++) {
    let inter = map(y, 0, height * 0.6, 0, 1);
    let c = lerpColor(color(colors.backgroundTop), color(colors.backgroundBottom), inter);
    stroke(c);
    line(0, y, width, y);
  }
  noStroke();
}

function drawHeader() {
  fill(colors.textDark);
  textStyle('700');
  textSize(52);
  text('Festejando Conexão', width / 2, 60);

  fill(colors.textGray);
  textSize(22);
  textStyle('400');
  text('Campo & Cidade', width / 2, 105);
}

function drawGroundImproved() {
  const groundY = height * 0.6;
  const groundH = height * 0.4;

  // Campo side ground vertical gradient
  noStroke();
  for (let y = 0; y <= groundH; y++) {
    let inter = map(y, 0, groundH, 0, 1);
    let c = lerpColor(color(colors.groundCampoStart), color(colors.groundCampoEnd), inter);
    stroke(c);
    line(0, groundY + y, width * 0.47, groundY + y);
  }
  noStroke();

  // Campo subtle vertical fine lines spaced every 20 px
  stroke('#f5a62333'); // very light orange
  strokeWeight(1.3);
  for (let x = 10; x < width * 0.47; x += 20) {
    line(x, groundY + 5, x, height);
  }

  noStroke();

  // City side ground vertical gradient
  for (let y = 0; y <= groundH; y++) {
    let inter = map(y, 0, groundH, 0, 1);
    let c = lerpColor(color(colors.groundCityStart), color(colors.groundCityEnd), inter);
    stroke(c);
    line(width * 0.53, groundY + y, width, groundY + y);
  }
  noStroke();

  // Fill city ground gap between campo and city sides to fix missing middle portion
  fill(lerpColor(color(colors.groundCampoEnd), color(colors.groundCityStart), 0.5));
  noStroke();
  rect(width * 0.47, groundY, width * 0.06, groundH);

  // Dividing line with rounded ends and thickness 4 px
  stroke(colors.dividingLine);
  strokeWeight(4);
  const lineX = width * 0.5;
  line(lineX, groundY, lineX, height);
  fill(colors.dividingLine);
  noStroke();
  ellipse(lineX, groundY, 6, 6);
  ellipse(lineX, height, 6, 6);
}

function drawCampoBooths() {
  const baseY = height * 0.6;
  const boothWidth = 160;
  const boothHeight = 130;
  const spacing = 70;
  const startX = 80;
  for (let i = 0; i < 3; i++) {
    let x = startX + i * (boothWidth + spacing);
    drawCampoBooth(x, baseY, boothWidth, boothHeight);
  }
  for (let c of characters) {
    // draw characters only near booths or near bottom campo
    if (c.boothIndex >= 0 || (c.boothIndex === -1 && c.yBase > height * 0.75)) {
      drawCharacter(c.x, c.y, 1);
    }
  }
}

function drawCampoBooth(x, baseY, w, h) {
  fill(colors.boothFill);
  drawingContext.shadowColor = colors.boothShadow;
  drawingContext.shadowBlur = 12;
  noStroke();
  rect(x, baseY - h, w, h, 20);
  drawingContext.shadowBlur = 0;
  fill(colors.boothRoof);
  beginShape();
  vertex(x, baseY - h);
  vertex(x + w / 2, baseY - h - 60);
  vertex(x + w, baseY - h);
  endShape(CLOSE);
  fill('rgba(255,255,255,0.7)');
  rect(x + 25, baseY - h + 55, w - 50, 50, 12);
  stroke('rgba(215, 115, 20, 0.6)');
  strokeWeight(3);
  for (let beamX = x + 40; beamX < x + w - 20; beamX += 25) {
    line(beamX, baseY - h + 55, beamX, baseY - h + 105);
  }
  noStroke();
  fill('#dc2626');
  ellipse(x + w * 0.3, baseY - h + 98, 18, 18);
  fill('#22c55e');
  ellipse(x + w * 0.53, baseY - h + 92, 12, 12);
  fill('#fde68a');
  ellipse(x + w * 0.73, baseY - h + 100, 15, 15);
}

function drawCharacter(x, y, scaleFactor = 1) {
  push();
  translate(x, y);
  scale(scaleFactor);
  noStroke();
  fill(colors.characterClothes);
  ellipse(0, -15, 20, 32);
  fill(colors.characterSkin);
  ellipse(0, -40, 18, 18);
  fill(colors.characterHair);
  arc(0, -44, 18, 22, 180, 360, CHORD);
  stroke(colors.characterClothes);
  strokeWeight(3);
  line(-10, -25, -18, -10);
  line(10, -25, 18, -10);
  stroke(colors.characterClothes);
  strokeWeight(4);
  line(-6, 0, -6, 22);
  line(6, 0, 6, 22);
  noStroke();
  fill('#374151');
  ellipse(-6, 26, 7, 4);
  ellipse(6, 26, 7, 4);
  pop();
}

function drawCityBuildings() {
  const baseY = height * 0.6;
  const bldgW = 130;
  const bldgH = 220;
  const spacing = 45;
  const startX = width * 0.55;

  for (let i = 0; i < 5; i++) {
    let x = startX + i * (bldgW + spacing);
    drawCityBuilding(x, baseY, bldgW, bldgH);
  }
}

function drawCityBuilding(x, baseY, w, h) {
  fill(colors.cityBuildingFill);
  drawingContext.shadowColor = colors.cityBuildingShadow;
  drawingContext.shadowBlur = 12;
  noStroke();
  rect(x, baseY - h, w, h, 18);
  drawingContext.shadowBlur = 0;

  fill(colors.cityBuildingRoof);
  beginShape();
  vertex(x, baseY - h);
  vertex(x + w / 2, baseY - h - 50);
  vertex(x + w, baseY - h);
  endShape(CLOSE);

  const cols = 4;
  const rows = 5;
  const winW = w * 0.18;
  const winH = h * 0.12;
  const paddingX = (w - cols * winW) / (cols + 1);
  const paddingY = (h * 0.75 - rows * winH) / (rows + 1);

  fill(colors.cityWindow);
  stroke(colors.cityWindowFrame);
  strokeWeight(1.8);
  for (let row = 0; row < rows; row++) {
    let y = baseY - h + 30 + paddingY + row * (winH + paddingY);
    for (let col = 0; col < cols; col++) {
      let xWin = x + paddingX + col * (winW + paddingX);
      rect(xWin, y, winW, winH, 4);
    }
  }
  noStroke();

  fill(colors.cityDoor);
  rect(x + w * 0.38, baseY - 65, w * 0.24, 65, 10);

  stroke(colors.cityBuildingShadow);
  strokeWeight(2);
  line(x + 8, baseY, x + w - 8, baseY);
  noStroke();
}

function drawStringWithFlags() {
  stroke(colors.stringColor);
  strokeWeight(3);
  line(100, 140, width - 100, 140);
  noStroke();
  for (let f of flags) {
    drawFlag(f.x, f.y, f.phase, f.size);
  }
}

function drawFlag(x, y, phase, sz) {
  let wave = sin(frameCount * 2 + phase) * 8;
  push();
  translate(x, y);
  rotate(wave * 0.22);
  stroke(colors.flagPole);
  strokeWeight(3);
  line(0, 0, 0, 45);
  noStroke();
  fill(colors.flagBlue);
  beginShape();
  vertex(0, 8);
  vertex(sz * 1.5 + wave * 0.7, 15 + wave * 0.5);
  vertex(0, 23);
  endShape(CLOSE);
  fill(colors.flagWhite);
  beginShape();
  vertex(sz * 0.3, 14 + wave * 0.1);
  vertex(sz * 1.1 + wave * 0.25, 16 + wave * 0.1);
  vertex(sz * 0.3, 20 + wave * 0.1);
  endShape(CLOSE);
  pop();
}

function drawAnimatedLights() {
  for (let i = 0; i < lights.length; i++) {
    let light = lights[i];
    light.timer += deltaTime * 0.05;
    if (light.timer > 50) {
      light.on = !light.on;
      light.timer = 0;
    }
    noStroke();
    const alpha = light.on ? 210 + 40 * sin(frameCount * 9 + i * 13) : 50;
    fill(252, 211, 77, alpha);
    ellipse(light.x, light.y, 7, 7);
  }
}

function updateCharacters() {
  for (let c of characters) {
    if (c.boothIndex >= 0) {
      // Walk within booth width limits
      c.x += c.direction * c.speed;
      let leftLimit = c.boothX + 30;
      let rightLimit = c.boothX + c.boothWidth - 30;
      if (c.x < leftLimit) {
        c.x = leftLimit;
        c.direction *= -1;
      } else if (c.x > rightLimit) {
        c.x = rightLimit;
        c.direction *= -1;
      }
    } else {
      // For characters near bottom campo, simple horizontal patrolling
      c.x += c.direction * c.speed;
      let leftLimit = 150;
      let rightLimit = 300;
      if (c.x < leftLimit) {
        c.x = leftLimit;
        c.direction *= -1;
      } else if (c.x > rightLimit) {
        c.x = rightLimit;
        c.direction *= -1;
      }
    }
    // Vertical bobbing recalculated fresh
    c.y = c.yBase + sin(frameCount * 8 + c.x) * 0.2;
  }
}

function drawStreet() {
  const baseY = height * 0.6;
  const streetTop = baseY + height * 0.15;
  const streetHeight = height * 0.1;
  noStroke();
  fill(colors.street);
  rect(width * 0.53, streetTop, width * 0.47, streetHeight, 15);

  // Dashed street center line
  stroke(colors.streetStripe);
  strokeWeight(4);
  drawingContext.setLineDash([20, 20]);
  line(width * 0.53, streetTop + streetHeight / 2, width, streetTop + streetHeight / 2);
  drawingContext.setLineDash([]);
  noStroke();
}

function drawCars() {
  for (let car of cars) {
    push();
    translate(car.x, car.y);
    noStroke();
    fill(car.colorBody);
    rect(-car.width / 2, -car.height / 2, car.width, car.height, 10);

    fill('#222');
    ellipse(-car.width * 0.3, car.height / 2 - 7, 18, 18);
    ellipse(car.width * 0.3, car.height / 2 - 7, 18, 18);

    fill(car.colorWindow);
    rect(-car.width * 0.4, -car.height / 2 + 8, car.width * 0.75, car.height * 0.4, 6);

    fill(255, 255, 255, 50);
    beginShape();
    vertex(-car.width / 2 + 6, -car.height / 2 + 4);
    vertex(car.width / 2 - 6, -car.height / 2 + 4);
    vertex(car.width / 2 - 12, -car.height / 2 + 12);
    vertex(-car.width / 2 + 12, -car.height / 2 + 12);
    endShape(CLOSE);

    pop();
  }
}

function updateCars() {
  const streetLeft = width * 0.53 + 10;
  const streetRight = width - 10;

  for (let car of cars) {
    car.x += car.speed * car.direction;

    if (car.x < streetLeft + car.width / 2) {
      car.x = streetLeft + car.width / 2;
      car.direction *= -1;
    } else if (car.x > streetRight - car.width / 2) {
      car.x = streetRight - car.width / 2;
      car.direction *= -1;
    }
  }
}

function drawBenchWithPeople() {
  // Bench position in bottom-left corner of campo ground, with subtle shadow and rounded corners
  const benchX = 100;
  const benchY = height * 0.92;
  const benchW = 140;
  const benchH = 30;
  const benchLegH = 20;
  
  // Bench seat
  drawingContext.shadowColor = colors.benchShadow;
  drawingContext.shadowBlur = 8;
  fill(colors.benchWood);
  rect(benchX, benchY, benchW, benchH, 10);

  // Bench legs
  drawingContext.shadowBlur = 0;
  fill(colors.benchLeg);
  rect(benchX + 15, benchY + benchH, 10, benchLegH, 3);
  rect(benchX + benchW - 25, benchY + benchH, 10, benchLegH, 3);

  // Two seated characters on bench (static, no bobbing or walking)
  // Sitting pose simplified - ellipse torso tilted back, head and legs bent
  drawSeatedCharacter(benchX + 45, benchY);
  drawSeatedCharacter(benchX + 95, benchY);
}

function drawSeatedCharacter(x, y) {
  push();
  translate(x, y);
  // torso leaning back
  fill(colors.characterClothes);
  ellipse(0, -10, 18, 26);
  // head
  fill(colors.characterSkin);
  ellipse(0, -32, 16, 16);
  // hair
  fill(colors.characterHair);
  arc(0, -36, 16, 20, 180, 360, CHORD);

  // arms resting on belly - subtle arcs
  stroke(colors.characterClothes);
  strokeWeight(3);
  noFill();
  arc(-6, -16, 18, 18, 40, 150);
  arc(6, -16, 18, 18, 30, 140);

  // legs bent, seated
  stroke(colors.characterClothes);
  strokeWeight(5);
  line(-8, 0, -8, 20);
  line(8, 0, 8, 20);
  noStroke();

  // shoes
  fill('#374151');
  ellipse(-8, 26, 7, 4);
  ellipse(8, 26, 7, 4);
  pop();
}

function mousePressed() {
  loop();
}

function mouseReleased() {
  noLoop();
}

