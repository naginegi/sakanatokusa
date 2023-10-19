<template>
    <div>
      <h1>游戏</h1>
      <canvas ref="gameCanvas" width="400" height="200"></canvas>
    </div>
  </template>
  
  <script>
  export default {
    mounted() {
      const canvas = this.$refs.gameCanvas;
      const ctx = canvas.getContext('2d');
  
      // 游戏元素
      const player = { x: 50, y: 100, width: 30, height: 30 };
      const obstacle = { x: 200, y: 100, width: 20, height: 20 };
  
      // 游戏循环
      function gameLoop() {
        // 清除画布
        ctx.clearRect(0, 0, canvas.width, canvas.height);
  
        // 绘制玩家和障碍物
        ctx.fillRect(player.x, player.y, player.width, player.height);
        ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
  
        // 碰撞检测
        if (
          player.x < obstacle.x + obstacle.width &&
          player.x + player.width > obstacle.x &&
          player.y < obstacle.y + obstacle.height &&
          player.y + player.height > obstacle.y
        ) {
          console.log('碰撞发生！');
          // 在这里执行碰撞后的操作
        }
  
        // 更新玩家的位置
        player.x += 1;
        if(player.x >= canvas.width){
            player.x =1;
                }
        requestAnimationFrame(gameLoop);
      }
  
      // 启动游戏循环
      gameLoop();
    },
  };
  </script>
  