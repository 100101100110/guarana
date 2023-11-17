# guarana
<!DOCTYPE html>
<html>
  <head>
    <title>Pong Game</title>
    <style>
      * {
        padding: 0;
        margin: 0;
      }
      
      canvas {
        background: #000;
        display: block;
        margin: 20px auto;
      }
    </style>
  </head>
  <body>
    <canvas id="pong" width="800" height="400"></canvas>
    
    <script>
      // Obtendo o elemento canvas e seu contexto
      const canvas = document.getElementById("pong");
      const context = canvas.getContext("2d");
      
      // Criando a raquete do jogador
      const player = {
        x: 0,
        y: canvas.height / 2 - 50,
        width: 10,
        height: 100,
        color: "#FFF"
      };
      
      // Criando a raquete do computador
      const computer = {
        x: canvas.width - 10,
        y: canvas.height / 2 - 50,
        width: 10,
        height: 100,
        color: "#FFF"
      };
      
      // Criando a bola
      const ball = {
        x: canvas.width / 2,
        y: canvas.height / 2,
        radius: 10,
        speed: 4,
        dx: 4,
        dy: 4,
        color: "#FFF"
      };
      
      // Desenhando as raquetes
      function drawRect(x, y, width, height, color) {
        context.fillStyle = color;
        context.fillRect(x, y, width, height);
      }
      
      // Desenhando a bola
      function drawCircle(x, y, radius, color) {
        context.fillStyle = color;
        context.beginPath();
        context.arc(x, y, radius, 0, Math.PI * 2, false);
        context.closePath();
        context.fill();
      }
      
      // Desenhando a rede
      function drawNet() {
        for (let i = 0; i <= canvas.height; i += 15) {
          drawRect(canvas.width / 2 - 1, i, 2, 10, "#FFF");
        }
      }
      
      // Atualizando a posição das raquetes
      function update() {
        player.y += 2;
        
        // Movendo a bola
        ball.x += ball.dx;
        ball.y += ball.dy;
        
        // Verificando colisões com as paredes
        if (ball.y + ball.radius > canvas.height || ball.y - ball.radius < 0) {
          ball.dy *= -1;
        }
        
        // Verificando colisão com as raquetes
        if (
          ball.x - ball.radius < player.x + player.width &&
          ball.y - ball.radius < player.y + player.height &&
          ball.y + ball.radius > player.y
        ) {
          ball.dx *= -1;
        }
        
        if (
          ball.x + ball.radius > computer.x &&
          ball.y - ball.radius < computer.y + computer.height &&
          ball.y + ball.radius > computer.y
        ) {
          ball.dx *= -1;
        }
      }
      
      // Renderizando todos os elementos do jogo
      function render() {
        // Limpar o canvas
        drawRect(0, 0, canvas.width, canvas.height, "#000");
        
        // Desenhar as raquetes
        drawRect(player.x, player.y, player.width, player.height, player.color);
        drawRect(computer.x, computer.y, computer.width, computer.height, computer.color);
        
        // Desenhar a bola
        drawCircle(ball.x, ball.y, ball.radius, ball.color);
        
        // Desenhar a rede
        drawNet();
      }
      
      // Função principal do jogo
      function game() {
        update();
        render();
      }
      
      // Loop do jogo
      function loop() {
        game();
        requestAnimationFrame(loop);
      }
      
      loop();
    </script>
  </body>
</html>