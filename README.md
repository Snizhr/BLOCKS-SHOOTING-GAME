# BLOCKS-SHOOTING-GAME
This repository contains a simple shooting game created as a practice project. The goal is to experiment with basic game development concepts such as player movement, shooting mechanics, scoring, and tracking a high score. It’s meant for learning and improving programming skills, not as a polished or professional release.


<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Shooting Game</title>
  <style>
    body { background: black; color: white; font-family: Arial; text-align: center; }
    canvas { background: grey; display:block; margin: 10px auto; border: 2px solid white; }
  </style>
</head>
<body>
  <h1>Shooting Game</h1>
  <p>Use Left/Right arrows to move, Space to shoot</p>
  <button onclick="startGame()">Start Game</button>
  <canvas id="gameCanvas" width="400" height="500"></canvas>
  <p id="score">Score: 0 | High Score: 0</p>

  <script>
    var canvas = document.getElementById("gameCanvas");
    var ctx = canvas.getContext("2d");
    var player = { x: 180, y: 450, w: 40, h: 20 };
    var bullets = [];
    var enemies = [];
    var score = 0;
    var highScore = 0;
    var keys = {};
    var gameInterval;

    document.addEventListener("keydown", function(e){ keys[e.code] = true; });
    document.addEventListener("keyup", function(e){ keys[e.code] = false; });

    function startGame(){
      bullets = [];
      enemies = [];
      score = 0;
      clearInterval(gameInterval);
      gameInterval = setInterval(updateGame, 30);
      updateScore();
    }

    function updateScore(){
      document.getElementById("score").textContent = "Score: " + score + " | High Score: " + highScore;
    }

    function updateGame(){
      ctx.clearRect(0,0,canvas.width,canvas.height);

      if(keys["ArrowLeft"] && player.x > 0){ player.x -= 5; }
      if(keys["ArrowRight"] && player.x < canvas.width - player.w){ player.x += 5; }

      if(keys["Space"]){
        if(bullets.length === 0 || bullets[bullets.length-1].y < player.y - 50){
          bullets.push({ x: player.x + player.w/2 - 2, y: player.y, w: 4, h: 10 });
        }
      }

      ctx.fillStyle = "blue";
      ctx.fillRect(player.x, player.y, player.w, player.h);

      for(var i=0; i<bullets.length; i++){
        bullets[i].y -= 7;
        ctx.fillStyle = "white";
        ctx.fillRect(bullets[i].x, bullets[i].y, bullets[i].w, bullets[i].h);
      }
      bullets = bullets.filter(b => b.y > 0);

      if(Math.random() < 0.03){
        enemies.push({ x: Math.random()*360, y: 0, w: 40, h: 20 });
      }

      for(var i=0; i<enemies.length; i++){
        enemies[i].y += 2;
        ctx.fillStyle = "red";
        ctx.fillRect(enemies[i].x, enemies[i].y, enemies[i].w, enemies[i].h);
      }

      for(var i=0; i<enemies.length; i++){
        for(var j=0; j<bullets.length; j++){
          if(bullets[j].x < enemies[i].x + enemies[i].w && bullets[j].x + bullets[j].w > enemies[i].x && bullets[j].y < enemies[i].y + enemies[i].h && bullets[j].y + bullets[j].h > enemies[i].y){
            enemies.splice(i,1);
            bullets.splice(j,1);
            score += 10;
            if(score > highScore){ highScore = score; }
            updateScore();
            break;
          }
        }
      }

      for(var i=0; i<enemies.length; i++){
        if(enemies[i].y > canvas.height){
          clearInterval(gameInterval);
          ctx.fillStyle = "yellow";
          ctx.font = "20px Arial";
          ctx.fillText("Game Over", 150, 250);
        }
      }
    }
  </script>
</body>
</html>
vvvv
