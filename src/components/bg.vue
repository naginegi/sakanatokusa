<script>
export default {
    data() {
        return {
            counter: 0,
            ten: ".",
            anime1:" ",
            anime2:" ",
            show:false,
            arr:[],
        };
    },
    created() {
        // 在组件创建后，开始一个周期性的操作
        setInterval(() => {
            this.counter += 1;
            //   console.log(this.counter);
            this.random1();
            // this.random2();
        }, 400); // 每隔 1 秒增加计数器的值
        setInterval(() => {
            this.counter += 1;
            //   console.log(this.counter);
            // this.random1();
            this.random2();
        }, 600); // 每隔 1 秒增加计数器的值
        //
        // window.addEventListener("keydown", this.handleSpaceKeyPress);
    },
    methods: {
        random1() {
            let ran = 0;
            ran = Math.floor(Math.random() * 40 + 20);
            this.anime1 = "bottom:" + ran + "%;";
        },
        random2() {
            let ran = 0;
            ran = Math.floor(Math.random() * 40 + 20);
            this.anime2 = "bottom:" + ran + "%;";
        },

        move() {
            // console.log("123")
        },

        // handleSpaceKeyPress(event) {
        //     if (event.key === "ArrowUp") {
        //         // let r = Math.floor(Math.random() * 10);
        //         // this.arr.length = r
        //         // 如果按下的键是空格键
        //         console.log("up");
                
        //     }
        //     if (event.key === "ArrowDown") {
        //         // let r = Math.floor(Math.random() * 10);
        //         // this.arr.length = r
        //         // 如果按下的键是空格键
        //         console.log("down");
                
        //     }
        //     if (event.key === "ArrowLeft") {
        //         // let r = Math.floor(Math.random() * 10);
        //         // this.arr.length = r
        //         // 如果按下的键是空格键
        //         console.log("left");
                
        //     }
        //     if (event.key === "ArrowRight") {
        //         // let r = Math.floor(Math.random() * 10);
        //         // this.arr.length = r
        //         // 如果按下的键是空格键
        //         console.log("Right");
                
        //     }
        // },
    },
    components: {},
};
</script>

<template>
    <p class="p1" :style="anime1">.</p>
    <p class="p2" :style="anime2">.</p>
    <!-- <div class="t" v-for=" i in arr" :key="index">
    </div> -->
    <!-- <i class="fa-solid fa-wind" :style="anime"></i> -->
    <!-- <i class="fa-solid fa-fish-fins" ></i> -->
</template>

<style lang="scss" scoped>
// .fa-solid {
//     rotate: 180deg;
//     font-size: 60pt;
//     position: fixed;
//     right: 0%;
//     bottom: 50%;
//     opacity: 30%;
//     // animation: wind 1s linear infinite;
// }

.p1{
    position: fixed;
    right: 0%;
    font-size: 50pt;
    animation: wind 0.4s linear infinite;
    
}
.p2{
    position: fixed;
    right: 0%;
    font-size: 50pt;
    opacity: 30%;
    animation: wind 1s linear infinite;
    
}

@keyframes wind {
    0% {
        right: 0%;
    }

    100% {
        right: 110%;
    }
}
</style>
