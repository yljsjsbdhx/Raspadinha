

<!DOCTYPE html>  <html lang="pt-BR">  
<head>  
  <meta charset="UTF-8" />  
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>  
  <title>Raspadinha Interativa</title>  
  <style>  
    body {  
      margin: 0;  
      display: flex;  
      justify-content: center;  
      align-items: center;  
      height: 100vh;  
      background: #f2f2f2;  
      font-family: Arial, sans-serif;  
    }  
    .container {  
      position: relative;  
      width: 400px;  
      height: 300px;  
      background-image: url('https://i.imgur.com/7ASlYYY.jpg');  
      background-size: cover;  
      border: 3px solid #000;  
      border-radius: 10px;  
      overflow: hidden;  
    }  
    .title {  
      position: absolute;  
      top: 10px;  
      left: 50%;  
      transform: translateX(-50%);  
      color: white;  
      font-size: 22px;  
      font-weight: bold;  
      z-index: 2;  
      text-shadow: 1px 1px 4px black;  
    }  
    canvas {  
      position: absolute;  
      top: 0;  
      left: 0;  
      z-index: 1;  
    }  
  </style>  
</head>  
<body>  
  <div class="container">  
    <div class="title">Nossa raspadinha especial🤍🤍</div>  
    <canvas id="scratchCanvas"></canvas>  
  </div>    <script>  
    const canvas = document.getElementById('scratchCanvas');  
    const ctx = canvas.getContext('2d');  
    const container = document.querySelector('.container');  
  
    canvas.width = container.clientWidth;  
    canvas.height = container.clientHeight;  
  
    // Preencher a camada de raspagem (cinza)  
    ctx.fillStyle = '#bbbbbb';  
    ctx.fillRect(0, 0, canvas.width, canvas.height);  
  
    ctx.globalCompositeOperation = 'destination-out';  
  
    function draw(x, y) {  
      ctx.beginPath();  
      ctx.arc(x, y, 20, 0, Math.PI * 2, false);  
      ctx.fill();  
    }  
  
    function getXY(e) {  
      const rect = canvas.getBoundingClientRect();  
      if (e.touches) {  
        return {  
          x: e.touches[0].clientX - rect.left,  
          y: e.touches[0].clientY - rect.top  
        };  
      } else {  
        return {  
          x: e.clientX - rect.left,  
          y: e.clientY - rect.top  
        };  
      }  
    }  
  
    let scratching = false;  
  
    // Eventos de mouse  
    canvas.addEventListener('mousedown', (e) => {  
      scratching = true;  
      const { x, y } = getXY(e);  
      draw(x, y);  
    });  
  
    canvas.addEventListener('mousemove', (e) => {  
      if (!scratching) return;  
      const { x, y } = getXY(e);  
      draw(x, y);  
    });  
  
    canvas.addEventListener('mouseup', () => scratching = false);  
    canvas.addEventListener('mouseleave', () => scratching = false);  
  
    // Eventos de toque (celular/tablet)  
    canvas.addEventListener('touchstart', (e) => {  
      scratching = true;  
      const { x, y } = getXY(e);  
      draw(x, y);  
    });  
  
    canvas.addEventListener('touchmove', (e) => {  
      if (!scratching) return;  
      e.preventDefault(); // evita rolagem enquanto raspa  
      const { x, y } = getXY(e);  
      draw(x, y);  
    });  
  
    canvas.addEventListener('touchend', () => scratching = false);  
  </script>  </body>  
</html>