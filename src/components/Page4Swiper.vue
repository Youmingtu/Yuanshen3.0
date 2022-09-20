<script setup>
import { onUnmounted, onMounted, ref } from "vue";

import Swiper, { Autoplay, Navigation, EffectFade, Mousewheel } from "swiper";
import "../../node_modules/swiper/swiper-bundle.min.css";
import "../../node_modules/swiper/swiper-bundle.min.js";
import "../../node_modules/swiper/swiper-bundle.js";

//各部件的自适应变量
let videoReplay1 = ref(null);
let videoReplay2 = ref(null);
let w21 = ref(0);
let h21 = ref(0);
let hArrow = ref("");
let lLeftArrow = ref("");
let tLeftArrow = ref("");
let lRightArrow = ref("");
let tRightArrow = ref("");
let wMonsterVideo = ref("");
let lMonsterVideo = ref("");

// 控制视频的切换显示
let videoPage = ref(0);
let videoPlay1 = ref(true);
let videoPlay2 = ref(false);

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

// 左下角按钮的图片
let monsterThumb1 = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/6ab9aff969213df3dbb8ff7cb12413e7_6045121217013881423.png",
);
let monsterThumb2 = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/e3039c9101b1404d83a10411dc640ff9_702624876991642173.png",
);

// 接收swiper方法
let page4SwiperEvent = ref();
let page4SwiperOP = ref({
    direction: "horizontal", //滑动方向
    speed: 300, //滑动速度
    slidesPerView: "auto", //滑动到贴合边缘
    mousewheel: false, //使用鼠标滚轮
    allowTouchMove: false, //不允许触屏拖动
    observer: true, // 监听
    observeParents: true, // 监听
    nested: true, //子组件标志
    resistanceRatio: 0,

    effect: "fade",
    fadeEffect: {
        crossFade: true,
    },
});

onMounted(() => {
    Swiper.use([Autoplay, Navigation, EffectFade, Mousewheel]);
    const page4Swiper = new Swiper(".page4Swiper", page4SwiperOP.value);

    page4SwiperEvent.value = page4Swiper; //复制以使用swiper方法

    slideZSY();
    window.addEventListener("resize", slideZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", slideZSY);
});

//自适应函数
const slideZSY = () => {
    w21.value = document.documentElement.clientWidth;
    h21.value = document.documentElement.clientHeight;
    if (w21.value / h21.value >= proportion) {
        //左侧图标选择
        hArrow.value = (0.15 * w21.value) / proportion + "px";
        lLeftArrow.value = 0.21 * w21.value + "px";
        tLeftArrow.value =
            (0.5 * w21.value) / proportion - (w21.value / proportion - h21.value) / 2 + "px";
        tRightArrow.value =
            (0.66 * w21.value) / proportion - (w21.value / proportion - h21.value) / 2 + "px";
        //中间视频模块
        wMonsterVideo.value = (421 / 1250) * w21.value + "px";
        lMonsterVideo.value = 0.501 * w21.value + "px";
    } else {
        //左侧图标选择
        hArrow.value = 0.15 * h21.value + "px";
        lLeftArrow.value =
            0.21 * h21.value * proportion - (h21.value * proportion - w21.value) / 2 + "px";
        tLeftArrow.value = 0.5 * h21.value + "px";

        tRightArrow.value = 0.66 * h21.value + "px";
        //中间视频模块
        wMonsterVideo.value = (421 / 1250) * h21.value * proportion + "px";
        lMonsterVideo.value =
            0.501 * h21.value * proportion - (h21.value * proportion - w21.value) / 2 + "px";
        //限制区域
        if (w21.value / h21.value <= wLimitProportion) {
            lMonsterVideo.value =
                0.501 * h21.value * proportion -
                (h21.value * proportion - h21.value * wLimitProportion) / 2 +
                "px";
            lLeftArrow.value =
                0.21 * h21.value * proportion -
                (h21.value * proportion - h21.value * wLimitProportion) / 2 +
                "px";
        } else {
        }
    }
};

//左下角两个缩略图(头像),当成左右箭头
const prevSlide = (e) => {
    if (videoPage.value == 1) {
        videoReplay1.value.currentTime = 0;
        monsterThumb1.value =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/6ab9aff969213df3dbb8ff7cb12413e7_6045121217013881423.png";
        monsterThumb2.value =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/e3039c9101b1404d83a10411dc640ff9_702624876991642173.png";
        page4SwiperEvent.value.slidePrev();
        videoPage.value = 0;
    }
};

const nextSlide = () => {
    if (videoPage.value == 0) {
        videoReplay2.value.currentTime = 0;
        monsterThumb2.value =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/fd17eefe42623e89a9e338765e7c16fd_9101852715119925808.png";
        monsterThumb1.value =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/c3c3b06d0b33f66b87380bcb82b4e2cd_6474136261850491278.png";
        page4SwiperEvent.value.slideNext();
        videoPage.value = 1;
    }
};

const leftArrowOver = () => {
    if (videoPage.value === 1) {
        monsterThumb1.value =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/6ab9aff969213df3dbb8ff7cb12413e7_6045121217013881423.png";
    }
};

const leftArrowLeave = () => {
    if (videoPage.value === 1) {
        monsterThumb1.value =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/c3c3b06d0b33f66b87380bcb82b4e2cd_6474136261850491278.png";
    }
};
const rightArrowOver = () => {
    if (videoPage.value === 0) {
        monsterThumb2.value =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/fd17eefe42623e89a9e338765e7c16fd_9101852715119925808.png";
    }
};

const rightArrowLeave = () => {
    if (videoPage.value === 0) {
        monsterThumb2.value =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/e3039c9101b1404d83a10411dc640ff9_702624876991642173.png";
    }
};
</script>

<template>
    <div class="swiper page4Swiper">
        <div class="swiper-wrapper">
            <div class="swiper-slide">
                <div
                    class="monsterVideo"
                    :style="{ left: lMonsterVideo, width: wMonsterVideo, height: wMonsterVideo }"
                >
                    <video
                        ref="videoReplay1"
                        class="monsterVideo1"
                        src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/efe5da0c73e89443c5fac804bcf204f2_4162764260578043412.mp4"
                        autoplay="autoplay"
                        loop
                        muted
                    ></video>
                    <img
                        class="slideWidth"
                        src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/3451bc691476999d4bbf735d6b88bab9_1700921785691078561.png"
                        alt=""
                        draggable="false"
                    />
                </div>
            </div>
            <div class="swiper-slide">
                <div
                    class="monsterVideo"
                    :style="{ left: lMonsterVideo, width: wMonsterVideo, height: wMonsterVideo }"
                >
                    <video
                        ref="videoReplay2"
                        class="monsterVideo1"
                        src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/7a6405adf6192d4c4dc94b6193e03466_3087300102722029178.mp4"
                        autoplay="autoplay"
                        loop
                        muted
                    ></video>
                    <img
                        class="slideWidth"
                        src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/ee72906057ea2a8f3a97c26964ff29f1_6022790122258011043.png"
                        alt=""
                        draggable="false"
                    />
                </div>
            </div>
        </div>
    </div>
    <!-- 前进后退按钮 -->
    <div
        class="leftArrow"
        @click="prevSlide"
        :style="{ left: lLeftArrow, top: tLeftArrow, height: hArrow, width: hArrow }"
    >
        <img
            @mouseover="leftArrowOver"
            @mouseleave="leftArrowLeave"
            class="leftArrowBg"
            :src="monsterThumb1"
            draggable="false"
            alt=""
        />
    </div>
    <div
        class="rightArrow"
        @click="nextSlide"
        :style="{ left: lLeftArrow, top: tRightArrow, height: hArrow, width: hArrow }"
    >
        <img
            @mouseover="rightArrowOver"
            @mouseleave="rightArrowLeave"
            class="rightArrowBg"
            :src="monsterThumb2"
            draggable="false"
            alt=""
        />
    </div>
</template>

<style scoped>
.page4Swiper {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
}

.leftArrow {
    position: absolute;
    left: 130px;
    top: 50%;
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
    top: 66%;
    width: 50px;
    height: 30px;
    z-index: 1;
}

.rightArrow .rightArrowBg {
    height: 100%;
}

.monsterVideo {
    position: absolute;
    top: 48.1%;
    left: 50.1%;
    width: 421px;
    height: 421px;
    border-radius: 50%;
    transform: translate(-50%, -50%);
    overflow: hidden;
    background-color: black;
}

.monsterVideo1 {
    position: absolute;
    top: 0;
    left: -42%;
    height: 100%;
}

.page4Swiper .swiper-wrapper .slideWidth {
    position: absolute;
    bottom: 0;
    width: 100%;
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
