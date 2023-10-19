<script>
import { Transition } from "vue";

export default {
    data() {
        return {
            gameInterval: null, //儲存遊戲貞數
            isGamePaused: false, //true就暫停遊戲
            keydo: false,
            keyup: false,
            i: 0,
            j: 0,
            page: 1,
            score: 0,
            wtype: 0,
            htype: 0,
            highest: localStorage.getItem('PlayerScore'),
            player: {
                x: 100,
                y: 430,
                width: 100,
                height: 100,
                speed: 10,
            },
            obstacle: {
                x: 1000,
                y: 460,
                width: 80,
                height: [80, 160, 240],
                speed: 10,
            },
        };
    },
    mounted() {
        this.gameCanvas = this.$refs.gameCanvas;
        this.ctx = this.gameCanvas.getContext("2d");
        this.startGame();
        window.addEventListener("keydown", this.KeyPress);
    },
    methods: {
        startGame() {
            this.gameInterval = setInterval(this.updateGameArea, 20); //遊戲貞數
            this.isGamePaused = false;
        },
        updateGameArea() {
            if (this.isGamePaused) return; //如果isGamePaused = true 暫停遊戲
            // this.keydow=false;
            this.clearCanvas(); //清除畫面
            this.updateObstacle(); //控制障礙物動作
            this.checkplayery();
            this.playeryup();
            this.scoreplus();
            this.checkCollision();//檢查碰撞
        },
        clearCanvas() {
            this.ctx.clearRect(0, 0, this.gameCanvas.width, this.gameCanvas.height);
        },
        updateObstacle() {
            this.obstacle.x -= this.obstacle.speed;
            if (this.obstacle.x + this.obstacle.width < 0) {
                this.obstacle.x = this.gameCanvas.width;
                this.obstacle.speed = Math.floor(Math.random() * 20 + 5);
                this.htype = Math.floor(Math.random() * 3)
                console.log("htype=" + this.htype)
                console.log("o.h=" + this.obstacle.height[this.htype])
            }
            this.drawObstacle();
        },
        drawObstacle() {
            this.ctx.fillStyle = "rgb(46, 70, 135)";
            this.ctx.fillRect(
                this.player.x,
                this.player.y,
                this.player.width,
                this.player.height
            );

            this.ctx.fillStyle = "rgb(135, 59, 46)";
            this.ctx.fillRect(
                this.obstacle.x,
                this.obstacle.y - this.htype * 80,
                this.obstacle.width,
                this.obstacle.height[this.htype],
            );
        },
        KeyPress(event) {
            if (event.key === " ") {
                this.keydo = true;
                this.keyup = true;

                // 如果按下的键是空
                // console.log(" ");

                // this.player.y -= this.player.speed*20
                this.playeryup();

            }
            this.keydo = false
        },
        playeryup() {
            if (!this.keydo && this.keyup) {
                this.player.y -= this.player.speed;
                this.j++;
                if (this.j > 30) {
                    this.j = 0;
                    this.keyup = false;
                }
            }

        },
        checkplayery() {
            if (!this.keydo && !this.keyup) {
                // console.log("y="+this.player.y)
                this.player.y += this.player.speed
                if (this.player.y >= 440) {
                    this.keydo = true
                }
            }
            // if(this.player.y <= gameCanvas.height)
        },
        checkCollision() {
            const playerLeft = this.player.x;
            const playerRight = this.player.x + this.player.width;
            const playerTop = this.player.y;
            const playerBottom = this.player.y + this.player.height;
            const obstacleLeft = this.obstacle.x;
            const obstacleRight = this.obstacle.x + this.obstacle.width;
            const obstacleTop = this.obstacle.y - this.htype * 80;
            const obstacleBottom = this.obstacle.y + this.obstacle.height[this.htype];

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
        scoreplus() {
            this.score++;
        },
        stopGame() {
            clearInterval(this.gameInterval);
            this.isGamePaused = true;
            this.page = 2;
            // alert('結束！');

            // this.obstacle.y = Math.floor(Math.random() * (this.gameCanvas.height - this.obstacle.height));
        },
        re() {
            // reset
            if (this.score > this.highest) {
                localStorage.setItem('PlayerScore', JSON.stringify(this.score));
                this.highest = this.score;
            }
            this.page = 1;
            this.startGame();
            this.player.x = 100;
            this.player.y = 430;
            this.score = 0;
            this.obstacle.x = this.gameCanvas.width;
            
        }

        // if (event.key === "ArrowUp") {
        //     // 如果按下的键是上
        //     console.log("up");

        // }
        // if (event.key === "ArrowDown") {
        //     // 如果按下的键是下
        //     console.log("down");

        // }
        // if (event.key === "ArrowLeft") {
        //     // 如果按下的键是左
        //     console.log("left");

        // }
        // if (event.key === "ArrowRight") {
        //     // 如果按下的键是右
        //     console.log("Right");

        // }
        // stop(i){
        //     clearInterval(this.timer)
        //     console.log(i)

        // },
        // getMouseCoordinates(event) {
        //     // const x = event.clientX;
        //     // const y = event.clientY;
        //     // this.sty = `left:${x - 50}px;top:${y - 250}px;`;
        //     // console.log(`x=${x},y=${y}.`)
        //     //  x=17,y=725.左下
        //     //  x=20,y=198.左上
        //     //  x=987,y=717.右下
        //     //  x=984,y=196.右上
        // },
        // inti() {
        //     const fishx = 50;
        //     const fishy = 450;
        //     this.sty = `left:${this.fishx}px;top:${this.fishy}px;`;
        // },
    },
};
</script>

<template>
    <div class="area">
        <canvas ref="gameCanvas" width="1325" height="542"></canvas>
        <div class="gover" v-if="page == 2">
            <h1>game-over</h1>
            <button type="button" @click="re">re</button>
        </div>
        <div class="score">
            <p>highest:</p>
            <p>{{ this.highest }}</p>
            <p>score:</p>
            <p>{{ this.score }}</p>
        </div>
        <!-- <div class="fa">
            <i class="fa-solid fa-fish"></i>
            <i class="fa-solid fa-seedling"></i>
        </div> -->
    </div>
    <!-- <div class="area" @click="getMouseCoordinates">
        <i class="fa-solid fa-fish-fins" :style="sty"></i>
    </div> -->
</template>

<style lang="scss" scoped>
.area {
    width: 100%;
    height: 100%;
    border: 1px solid;
    position: relative;

    .gover {
        width: 12%;
        height: 50%;
        background-color: white;
        display: flex;
        flex-direction: column;
        align-items: center;
        position: absolute;
        top: 25%;
        left: 43%;

        button {
            width: 40%;
            height: 15%;
        }
    }

    .score {
        display: flex;
        font-size: 18pt;
        position: absolute;
        top: 1%;
        right: 5%;
    }

    .fa{
        width: auto;
        height: auto;
        position: absolute;
        top: 82%;
        left: 10%;
        .fa-solid {
            font-size: 72pt;
        }
    }
}

// .area {
//     width: 100%;
//     height: 100%;
//     border: 1px solid;
//     position: relative;

//     .fa-solid {
//         width: 90px;
//         height: 90px;
//         font-size: 60pt;
//         position: absolute;
//         // left: 35px;
//         // top: 20px;

//         // animation: wind 1s linear infinite;
//     }
// }
</style>
