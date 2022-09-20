<script setup>
import { onUnmounted, onMounted, ref } from "vue";

import Swiper, { Autoplay, Navigation, EffectFade, Mousewheel } from "swiper";
import "../../node_modules/swiper/swiper-bundle.min.css";
import "../../node_modules/swiper/swiper-bundle.min.js";
import "../../node_modules/swiper/swiper-bundle.js";

let videoReplay = ref(null); //控制视频的重新播放

//各部件的自适应变量
let w2 = ref(0);
let h2 = ref(0);
let hArrow = ref("");
let lLeftArrow = ref("");
let tLeftArrow = ref("");
let lRightArrow = ref("");
let tRightArrow = ref("");
let wMechanismVideo = ref("");
let hMechanismVideo = ref("");
let lMechanismVideo = ref("");
let tMechanismVideo = ref("");
let hpage5VideoBg = ref("");
let lpage5VideoBg = ref("");
let tpage5VideoBg = ref("");
let wrightButton = ref("");
let lrightButton = ref("");
let trightButton = ref("");

//右侧切换的按钮时的临时变量
let buttonBgTemp = ref("");

//视频数据以及正在播放的视频和对应按钮的激活状态
let videoPage = ref(0);
let mechanismVideos = ref([
    {
        id: 0,
        playState: true,
        videoURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/dbf1cb7bfa6abae5cd4a930e37e26bdb_633285267510277095.mp4",
        videoBgURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/943cefef41843e369b01515ba108b810_3964205697203520428.png",
    },
    {
        id: 1,
        playState: false,
        videoURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/d90de20689297916ad197f6f6f48bb49_3700354761240458221.mp4",
        videoBgURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/6d48a11a14178a0b1e9ed66fbb9d6221_5761790032206359904.png",
    },
    {
        id: 2,
        playState: false,
        videoURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/b93ad5de8cd1515681501d09a12c443f_2184563095678811226.mp4",
        videoBgURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/9a5fe18b430f7cf62bffc2ba85b04abc_4107283183600362138.png",
    },
    {
        id: 3,
        playState: false,
        videoURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/b41dc3cba12303cfab4bf130930eebc4_6662504832412034066.mp4",
        videoBgURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/c3e19fc6b54298ea463a3a9dc19f0799_1876450670358774054.png",
    },
]);
let buttonBg = ref([
    {
        id: 0,
        activeState: true,
        buttonBgURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/314b9e59432a7a3bfb78160a023c17da_5938626122705750178.png",
    },
    {
        id: 1,
        activeState: false,
        buttonBgURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/df8329d3d35b242fafda63ddb7ff1c4d_3401899369002183562.png",
    },
    {
        id: 2,
        activeState: false,
        buttonBgURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/d6e283cde5608163468935c8795f6edf_2386414728935389040.png",
    },
    {
        id: 3,
        activeState: false,
        buttonBgURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/d2523c98114bd158df712a618a72fca6_6127422130814462108.png",
    },
]);
let buttonActiveBg = ref([
    {
        id: 0,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/314b9e59432a7a3bfb78160a023c17da_5938626122705750178.png",
    },
    {
        id: 1,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/40eb533bddc5143a30e81832802ce737_6726779099549738832.png",
    },
    {
        id: 2,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/7ae8be9f51225073be2c2984f13491f0_6273669275458931278.png",
    },
    {
        id: 3,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/860436c028a4b849f9fbea8acf946dd4_1264088770772427967.png",
    },
]);

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

//swiper方法和设置
let page5SwiperEvent = ref();
let page5SwiperOP = ref({
    direction: "horizontal", //滑动方向
    speed: 10, //滑动速度
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
    const page5Swiper = new Swiper(".page5Swiper", page5SwiperOP.value);

    page5SwiperEvent.value = page5Swiper; //复制以使用swiper方法

    slideZSY();
    window.addEventListener("resize", slideZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", slideZSY);
});

//自适应函数
const slideZSY = () => {
    w2.value = document.documentElement.clientWidth;
    h2.value = document.documentElement.clientHeight;
    if (w2.value / h2.value >= proportion) {
        //左侧图标选择
        hArrow.value = (0.15 * w2.value) / proportion + "px";
        lLeftArrow.value = 0.21 * w2.value + "px";
        tLeftArrow.value =
            (0.5 * w2.value) / proportion - (w2.value / proportion - h2.value) / 2 + "px";
        tRightArrow.value =
            (0.66 * w2.value) / proportion - (w2.value / proportion - h2.value) / 2 + "px";
        //中间视频模块
        wMechanismVideo.value = (480 / 1250) * w2.value + "px";
        hMechanismVideo.value = 0.505 * (480 / 1250) * w2.value + "px";
        lMechanismVideo.value = 0.5005 * w2.value + "px";
        tMechanismVideo.value =
            (0.4385 * w2.value) / proportion - (w2.value / proportion - h2.value) / 2 + "px";
        //视频的背景图片
        hpage5VideoBg.value = (0.85 * w2.value) / proportion + "px";
        tpage5VideoBg.value =
            (0.46 * w2.value) / proportion - (w2.value / proportion - h2.value) / 2 + "px";
        lpage5VideoBg.value = 0.4925 * w2.value + "px";
        //右侧按钮
        wrightButton.value = (95 / 1250) * w2.value + "px";
        lrightButton.value = (930 / 1250) * w2.value + "px";
        trightButton.value =
            ((135 / 540) * w2.value) / proportion - (w2.value / proportion - h2.value) / 2 + "px";
    } else {
        //左侧图标选择
        hArrow.value = 0.15 * h2.value + "px";
        lLeftArrow.value =
            0.21 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        tLeftArrow.value = 0.5 * h2.value + "px";

        tRightArrow.value = 0.66 * h2.value + "px";
        //中间视频模块
        wMechanismVideo.value = (480 / 1250) * h2.value * proportion + "px";
        hMechanismVideo.value = 0.505 * (480 / 1250) * h2.value * proportion + "px";
        lMechanismVideo.value =
            0.5005 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        tMechanismVideo.value = 0.4385 * h2.value + "px";
        //视频的背景图片
        hpage5VideoBg.value = 0.85 * h2.value + "px";
        tpage5VideoBg.value = 0.46 * h2.value + "px";
        lpage5VideoBg.value =
            0.4925 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        //右侧按钮
        wrightButton.value = (95 / 1250) * h2.value * proportion + "px";
        lrightButton.value =
            (930 / 1250) * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        trightButton.value = (135 / 540) * h2.value + "px";
        //限制区域
        if (w2.value / h2.value <= wLimitProportion) {
            lMechanismVideo.value =
                0.5005 * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
            lrightButton.value =
                (930 / 1250) * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
            lpage5VideoBg.value =
                0.4925 * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
        } else {
        }
    }
};

//右侧按钮点击时,改变激活状态和按钮图片,以及鼠标经过时的改变
const buttonClick = (id) => {
    if (buttonBg.value[id].activeState != true) {
        console.log("点击了id:", id);
        for (let i = 0; i < buttonBg.value.length; i++) {
            buttonBg.value[i].activeState = false;
            mechanismVideos.value[i].playState = false;
        }
        buttonBg.value[id].activeState = true;
        buttonBgChange(id);
        mechanismVideos.value[id].playState = true;
        videoReplay.value[id].currentTime = 0;
        page5SwiperEvent.value.slideTo(id);
    }
};

const buttonBgChange = (id) => {
    for (let i = 0; i < buttonBg.value.length; i++) {
        switch (i) {
            case 0:
                buttonBg.value[0].buttonBgURL =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/fbbbedfe71454a3afca3505d13575a66_6553441828236895503.png";

                break;
            case 1:
                buttonBg.value[1].buttonBgURL =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/df8329d3d35b242fafda63ddb7ff1c4d_3401899369002183562.png";

                break;
            case 2:
                buttonBg.value[2].buttonBgURL =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/d6e283cde5608163468935c8795f6edf_2386414728935389040.png";

                break;
            case 3:
                buttonBg.value[3].buttonBgURL =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/d2523c98114bd158df712a618a72fca6_6127422130814462108.png";

                break;

            default:
                break;
        }
    }
    switch (id) {
        case 0:
            buttonBg.value[0].buttonBgURL = buttonActiveBg.value[0].url;

            break;
        case 1:
            buttonBg.value[1].buttonBgURL = buttonActiveBg.value[1].url;

            break;
        case 2:
            buttonBg.value[2].buttonBgURL = buttonActiveBg.value[2].url;

            break;
        case 3:
            buttonBg.value[3].buttonBgURL = buttonActiveBg.value[3].url;

            break;

        default:
            break;
    }
};

const buttonBgOver = (id) => {
    if (buttonBg.value[id].activeState != true) {
        buttonBgTemp.value = buttonBg.value[id].buttonBgURL;
        buttonBg.value[id].buttonBgURL = buttonActiveBg.value[id].url;
    }
};

const buttonBgLeave = (id) => {
    if (buttonBg.value[id].activeState != true) {
        buttonBg.value[id].buttonBgURL = buttonBgTemp.value;
    }
};
</script>

<template>
    <div class="swiper page5Swiper">
        <div class="swiper-wrapper">
            <div class="swiper-slide" v-for="item in mechanismVideos" :key="item.id">
                <img
                    :style="{ height: hpage5VideoBg, top: tpage5VideoBg, left: lpage5VideoBg }"
                    class="videoBg"
                    :src="item.videoBgURL"
                    alt=""
                    draggable="false"
                />
                <div
                    class="MechanismVideo"
                    :style="{
                        width: wMechanismVideo,
                        height: hMechanismVideo,
                        left: lMechanismVideo,
                        top: tMechanismVideo,
                    }"
                >
                    <video
                        ref="videoReplay"
                        class="MechanismVideo1"
                        :src="item.videoURL"
                        autoplay="autoplay"
                        loop
                        muted
                    ></video>
                </div>
            </div>
        </div>
    </div>
    <!-- 右侧切换按钮 -->
    <div
        class="rightButton"
        :style="{
            width: wrightButton,
            left: lrightButton,
            top: trightButton,
        }"
    >
        <img
            @click="buttonClick(item.id)"
            @mouseover="buttonBgOver(item.id)"
            @mouseleave="buttonBgLeave(item.id)"
            class="rightButtonBg"
            v-for="item in buttonBg"
            :key="item.id"
            :src="item.buttonBgURL"
            draggable="false"
            alt=""
        />
    </div>
</template>

<style scoped>
.page5Swiper {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
}

.rightButton {
    position: absolute;
    top: 135px;
    left: 930px;
    width: 95px;
    /* background-color: rgba(255, 0, 0, 0.2); */
    z-index: 1;
}

.rightButtonBg {
    width: 100%;
}

.MechanismVideo {
    position: absolute;
    top: 43.85%;
    left: 50.1%;
    /* width: 500px; */
    height: 300px;
    overflow: hidden;
    transform: translate(-50%, -50%);
    background-color: black;
}

.MechanismVideo1 {
    position: absolute;
    top: -61%;
    transform: translate(0, 50%);
    width: 100%;
}

.videoBg {
    position: absolute;
    top: 46%;
    left: 49%;
    height: 85%;
    transform: translate(-50%, -50%);
    z-index: -1;
    /* background-color: rgba(255, 0, 0, 0.2); */
}

.page5Swiper .swiper-wrapper .slideWidth {
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
