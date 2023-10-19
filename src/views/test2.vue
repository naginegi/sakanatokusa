<template>
    <div>
        <h2>游戏示例</h2>
        <canvas ref="gameCanvas" width="400" height="400"></canvas>
        <button @click="startGame">开始游戏</button>
        <button @click="pauseGame">暂停游戏</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            gameInterval: null, // 用于存储游戏循环的定时器
            isGamePaused: false,
            player: {
                x: 100,
                y: 50,
                width: 20,
                height: 20,
                speed: 5,
            },
            obstacle: {
                x: 200,
                y: 200,
                width: 30,
                height: 30,
            },
        };
    },
    mounted() {
        this.gameCanvas = this.$refs.gameCanvas;
        this.ctx = this.gameCanvas.getContext('2d');
        this.startGame();
        window.addEventListener("keydown", this.KeyPress);
    },
    methods: {
        updateGameArea() {
            if (this.isGamePaused) return;

            this.clearCanvas();
            this.updatePlayer();
            this.updateObstacle();
            this.checkCollision();
        },
        clearCanvas() {
            this.ctx.clearRect(0, 0, this.gameCanvas.width, this.gameCanvas.height);
        },
        updatePlayer() {
            if (this.player.x < this.gameCanvas.width - this.player.width) {
                // this.player.x += this.player.speed;
            }

        },
        KeyPress(event) {
            if (event.key === 'w') {
                // 如果按下的键是上
                console.log("up");
                this.player.y = this.player.y-10

            }
            if (event.key === "s") {
                // 如果按下的键是下
                console.log("down");
                this.player.y =this.player.y+10

            }
            if (event.key === "a") {
                // 如果按下的键是左
                console.log("left");
                this.player.x = this.player.x-10

            }
            if (event.key === "d") {
                // 如果按下的键是右
                console.log("Right");
                this.player.x = this.player.x+10

            }
        },
        updateObstacle() {
            //等速往左運動，右邊碰到左邊邊線，重整
            this.obstacle.x -= this.player.speed;
            if (this.obstacle.x + this.obstacle.width < 0) {
                this.obstacle.x = this.gameCanvas.width;
                this.obstacle.y = Math.floor(Math.random() * (this.gameCanvas.height - this.obstacle.height));
            }
            this.drawObstacle();
        },
        drawObstacle() {
            this.ctx.fillStyle = 'blue';
            this.ctx.fillRect(this.player.x, this.player.y, this.player.width, this.player.height);
            this.ctx.fillStyle = 'red';
            this.ctx.fillRect(this.obstacle.x, this.obstacle.y, this.obstacle.width, this.obstacle.height);
        },
        checkCollision() {
            const playerLeft = this.player.x;
            const playerRight = this.player.x + this.player.width;
            const playerTop = this.player.y;
            const playerBottom = this.player.y + this.player.height;
            const obstacleLeft = this.obstacle.x;
            const obstacleRight = this.obstacle.x + this.obstacle.width;
            const obstacleTop = this.obstacle.y;
            const obstacleBottom = this.obstacle.y + this.obstacle.height;

            //碰撞概念
            if (
                playerRight > obstacleLeft &&
                playerLeft < obstacleRight &&
                playerBottom > obstacleTop &&
                playerTop < obstacleBottom
            ) {
                this.stopGame();
            }
        },
        startGame() {
            this.gameInterval = setInterval(this.updateGameArea, 20); // 设置自定义帧数
            this.isGamePaused = false;
        },
        pauseGame() {
            this.isGamePaused = true;
        },
        stopGame() {
            clearInterval(this.gameInterval);
            this.isGamePaused = false;
            // alert('結束！');
            // 重置游戏数据，如果需要的话
            this.player.x = 50;
            this.player.y = 50;
            this.obstacle.x = this.gameCanvas.width;
            this.obstacle.y = Math.floor(Math.random() * (this.gameCanvas.height - this.obstacle.height));
        },
    },
};
</script>
