// Player positions and velocities
var x1 = 200, y1 = 350, vx1 = 0;
var x2 = 200, y2 = 50, vx2 = 0;

// Key tracking
var keys = {};

// Indicator data for both players
var indicator1 = { angle: 0, direction: -1, range: 60, speed: 3 }; // swings from -90 to +90 (centered on 90°)
var indicator2 = { angle: 0, direction: 1, range: 60, speed: 3 }; // same for top

var toRadians = function(deg) {
return deg \* Math.PI / 180;
};

var state = "menu";

mousePressed = function() {
// If on menu screen and "Play" button is clicked
if (state === "menu") {
if (mouseX > 150 && mouseX < 250 && mouseY > 220 && mouseY < 270) {
state = "game"; // start the game
}
}
};

var drawMenu = function() {
background(220);

    // Title
    fill(0);
    textAlign(CENTER, CENTER);
    textSize(36);
    text("Mundo Dodgeball", 200, 100);

    // Play button
    fill(0, 150, 255);
    rect(150, 220, 100, 50, 10); // rounded button

    fill(255);
    textSize(20);
    text("Play", 200, 245);

};

keyPressed = function() {
keys[keyCode] = true;
};

keyReleased = function() {
keys[keyCode] = false;
};

// Movement logic
var updateMovement = function() {
// Bottom player (A and D)
vx1 = keys[65] ? -2 : keys[68] ? 2 : 0;

    // Top player (Arrow keys)
    vx2 = keys[LEFT] ? -2 : keys[RIGHT] ? 2 : 0;

    x1 = constrain(x1 + vx1, 25, 375);
    x2 = constrain(x2 + vx2, 25, 375);

};

// Indicator angle update
var updateIndicator = function(ind) {
ind.angle += ind.speed _ ind.direction;
if (ind.angle > ind.range || ind.angle < -ind.range) {
ind.direction _= -1;
}
};

// Draw swinging line
var drawIndicator = function(baseX, baseY, ind, baseAngle, colorVal) {
var angleDeg = baseAngle + ind.angle; // baseAngle + swing offset
var angleRad = toRadians(angleDeg);
var tipX = baseX + Math.cos(angleRad) _ 60;
var tipY = baseY + Math.sin(angleRad) _ 60;

    stroke(colorVal);
    strokeWeight(3);
    line(baseX, baseY, tipX, tipY);

};

draw = function() {
if (state === "menu") {
drawMenu();
} else if (state === "game"){
background(255);

    updateMovement();

    // Draw players
    fill(0); ellipse(x1, y1, 50, 50);
    fill(100); ellipse(x2, y2, 50, 50);

    updateIndicator(indicator1);
    updateIndicator(indicator2);

    drawIndicator(x1, y1, indicator1, 270, 0);   // Bottom: swing centered at 180°
    drawIndicator(x2, y2, indicator2, 90, 100);
    }

};
