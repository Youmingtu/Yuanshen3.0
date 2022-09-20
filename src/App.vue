<script setup>
import Page1 from "./views/Page1.vue";
import Page2 from "./views/Page2.vue";
import Page3 from "./views/Page3.vue";
import Page4 from "./views/Page4.vue";
import Page5 from "./views/Page5.vue";
import Page6 from "./views/Page6.vue";
import Page7 from "./views/Page7.vue";
import Page8 from "./views/Page8.vue";

import { onBeforeMount, onMounted, ref } from "vue";

import Swiper, { Pagination, Mousewheel } from "swiper";
import "../node_modules/swiper/swiper-bundle.min.css";
import "../node_modules/swiper/swiper-bundle.min.js";
import "../node_modules/swiper/swiper-bundle.js";

let mouseWheel = ref(true); //控制鼠标的滚动
let swiperAble = ref(); //使用swiper方法

//当点击某个部件时禁用swiper（不能滚动到下一页或上一页）
const MWControl = (value) => {
    if (!value) swiperAble.value.disable();
    else swiperAble.value.enable();
};
const playVideo = (value) => {
    if (value) swiperAble.value.disable();
    else swiperAble.value.enable();
};
const canScroll = (value) => {
    if (!value) swiperAble.value.disable();
    else swiperAble.value.enable();
};

let swiperOptions = ref({}); //swiper设置

onBeforeMount(() => {
    swiperOptions.value = {
        initialSlide: 0, //默认显示第0页
        direction: "vertical", //滑动方向
        speed: 400, //滑动速度
        slidesPerView: "auto", //滑动到贴合边缘
        mousewheel: true, //使用鼠标滚轮
        observer: true, // 监听
        observeParents: true, // 监听
        allowTouchMove: false, //不允许触屏拖动

        //分页器
        // pagination: {
        //     el: ".swiper-pagination",
        //     clickable: true,
        // },
    };
});

onMounted(() => {
    Swiper.use([Pagination, Mousewheel]);
    const mySwiper = new Swiper(".swiperAll", swiperOptions.value); //创建swiper

    swiperAble.value = mySwiper; //复制以使用swiper方法

    // 使用swiper方法
    mySwiper.on("transitionStart", (swiper) => {
        setTimeout(() => {}, 10);
    });
});
</script>

<template>
    <div class="app-body">
        <div class="swiper swiperAll">
            <div class="swiper-wrapper">
                <div class="swiper-slide over">
                    <page1 @MWControl="MWControl"></page1>
                </div>
                <div class="swiper-slide over">
                    <Page2></Page2>
                </div>
                <div class="swiper-slide over">
                    <Page3 @playVideo="playVideo"></Page3>
                </div>
                <div class="swiper-slide over"><Page4></Page4></div>
                <div class="swiper-slide over"><Page5></Page5></div>
                <div class="swiper-slide over"><Page6></Page6></div>
                <div class="swiper-slide over"><Page7 @canScroll="canScroll"></Page7></div>
                <div class="swiper-slide over footer"><Page8></Page8></div>
            </div>
            <!-- 如果需要分页器 -->
            <!-- <div class="swiper-pagination"></div> -->
        </div>
    </div>
</template>

<style scoped>
.app-body {
    width: 100vw;
    height: 100vh;
    /* border: 2px solid blue; */
}

.swiper {
    width: 100%;
    height: 100%;
    /* border: 2px solid blue; */
}

.footer {
    width: 100%;
    height: 350px;
    background-color: black;
}

.over {
    overflow: hidden;
}
</style>
