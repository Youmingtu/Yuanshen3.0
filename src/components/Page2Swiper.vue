<script setup>
import { onUnmounted, onMounted, ref } from "vue";

import Swiper, { Autoplay, Navigation, EffectFade, Mousewheel } from "swiper";
import "../../node_modules/swiper/swiper-bundle.min.css";
import "../../node_modules/swiper/swiper-bundle.min.js";
import "../../node_modules/swiper/swiper-bundle.js";

//自适应变量
let w21 = ref(0);
let h21 = ref(0);
let hArrow = ref("");
let lLeftArrow = ref("");
let lRightArrow = ref("");

//左右按钮图片
const leftArrowChangeBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/e4d452f3c4a351b2b5a7116f9a225bc1_5063615372114385026.png",
);
const rightArrowChangeBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/c1d9fb6fd9be05a5c19e8b0a7c5f57b5_7050763259058962930.png",
);

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

// swiper设置
let page2SwiperEvent = ref();
let page2SwiperOP = ref({
    direction: "horizontal", //滑动方向
    speed: 300, //滑动速度
    loop: true, //循环
    slidesPerView: "auto", //滑动到贴合边缘
    mousewheel: false, //使用鼠标滚轮
    allowTouchMove: true, //不允许触屏拖动
    observer: true, // 监听
    observeParents: true, // 监听
    nested: true, //子组件标志
    resistanceRatio: 0,

    autoplay: {
        delay: 3000, //自动轮播时间间隔（单位ms）
        disableOnInteraction: false, //触碰后不停止自动切换
        pauseOnMouseEnter: false, //鼠标放上去时是否停止自动切换
    },

    effect: "fade", //切换效果--渐入渐出
    fadeEffect: {
        crossFade: true, //防止slide堆叠
    },

    //左右按钮
    navigation: {
        nextEl: ".swiper-button-next",
        prevEl: ".swiper-button-prev",
    },
});

onMounted(() => {
    Swiper.use([Autoplay, Navigation, EffectFade, Mousewheel]);
    const page2Swiper = new Swiper(".page2Swiper", page2SwiperOP.value);

    page2SwiperEvent.value = page2Swiper; //复制以使用swiper方法

    slideZSY();
    //窗口监听并执行自适应函数
    window.addEventListener("resize", slideZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", slideZSY);
});

// 自适应函数
const slideZSY = () => {
    w21.value = document.documentElement.clientWidth;
    h21.value = document.documentElement.clientHeight;
    if (w21.value / h21.value >= proportion) {
        hArrow.value = (0.069 * w21.value) / proportion + "px";
        lLeftArrow.value = 0.13 * w21.value + "px";
        lRightArrow.value = 0.305 * w21.value + "px";
    } else {
        hArrow.value = 0.069 * h21.value + "px";
        lLeftArrow.value = 0.13 * h21.value * proportion + "px";
        lRightArrow.value = 0.305 * h21.value * proportion + "px";
        //限制区域
        if (w21.value / h21.value <= wLimitProportion) {
        } else {
        }
    }
};

//“上一页”按钮
const prevSlide = () => {
    console.log("prevent");
    page2SwiperEvent.value.slidePrev();
};
//“下一页”按钮
const nextSlide = () => {
    console.log("next");
    page2SwiperEvent.value.slideNext();
};

//以下是左右箭头的mouseover和mouseleave改变按钮图片
const leftArrowOver = () => {
    leftArrowChangeBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/1eea5c34158521ab68648b0e9da24f9c_2474559445104877826.png";
};

const leftArrowLeave = () => {
    leftArrowChangeBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/e4d452f3c4a351b2b5a7116f9a225bc1_5063615372114385026.png";
};

const rightArrowOver = () => {
    rightArrowChangeBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/2fb978d6f6ca8406de6350f5905138c8_3832078844698218457.png";
};

const rightArrowLeave = () => {
    rightArrowChangeBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/c1d9fb6fd9be05a5c19e8b0a7c5f57b5_7050763259058962930.png";
};
</script>

<template>
    <!-- 中间部分 -->
    <div class="swiper page2Swiper">
        <div class="swiper-wrapper">
            <div class="swiper-slide">
                <img
                    class="slideHeight"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/8e049f825dfde54c2a97936a89a0b4a6_915625744135471261.png"
                    alt=""
                />
            </div>
            <div class="swiper-slide">
                <img
                    class="slideHeight"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/a6d804652ae021889cbb23e88f5b017f_1192866295123615419.png"
                    alt=""
                />
            </div>
            <div class="swiper-slide">
                <img
                    class="slideHeight"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/1cc11c43fa854027fce51a8bb5cc4be6_8484775255676217721.png"
                    alt=""
                />
            </div>
            <div class="swiper-slide">
                <img
                    class="slideHeight"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/24/19992110045b215c02569c3cdec3d292_8596628525943575354.png"
                    alt=""
                />
            </div>
            <div class="swiper-slide">
                <img
                    class="slideHeight"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/960c16471d8fe40416f8654ca0f09157_7686496681378036192.png"
                    alt=""
                />
            </div>
            <div class="swiper-slide">
                <img
                    class="slideHeight"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/d9bb1d75270d77c39e659f36f4b71128_2773516408265374348.png"
                    alt=""
                />
            </div>
            <div class="swiper-slide">
                <img
                    class="slideHeight"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/b32f443f2d5538e26e43cf3fcc96537d_5377100219471900795.png"
                    alt=""
                />
            </div>
            <div class="swiper-slide">
                <img
                    class="slideHeight"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/c9adb8b12303cda17a92b3f1c566eed2_5647992399153593476.png"
                    alt=""
                />
            </div>
        </div>
    </div>
    <!-- 前进后退按钮 -->
    <div class="leftArrow" @click="prevSlide" :style="{ left: lLeftArrow, height: hArrow }">
        <img
            @mouseover="leftArrowOver"
            @mouseleave="leftArrowLeave"
            class="leftArrowBg"
            :src="leftArrowChangeBg"
            draggable="false"
            alt=""
        />
    </div>
    <div class="rightArrow" @click="nextSlide" :style="{ left: lRightArrow, height: hArrow }">
        <img
            @mouseover="rightArrowOver"
            @mouseleave="rightArrowLeave"
            class="rightArrowBg"
            :src="rightArrowChangeBg"
            draggable="false"
            alt=""
        />
    </div>
</template>

<style scoped>
.page2Swiper {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
}

.leftArrow {
    position: absolute;
    left: 130px;
    top: 92%;
    width: 50px;
    height: 30px;
    z-index: 1;
}

.leftArrow .leftArrowBg {
    height: 100%;
}

.rightArrow {
    position: absolute;
    left: 295px;
    top: 92%;
    width: 50px;
    height: 30px;
    z-index: 1;
}

.rightArrow .rightArrowBg {
    height: 100%;
}

.page2Swiper .swiper-wrapper .slideHeight {
    height: 100%;
}
/* .s1 {
    background-color: greenyellow;
}
.s2 {
    background-color: pink;
}
.s3 {
    background-color: skyblue;
} */
</style>
