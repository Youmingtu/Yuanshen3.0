<script setup>
import { onUnmounted, onMounted, ref, watch } from "vue";

import page3Swiper from "../components/Page3Swiper.vue";

const emit = defineEmits(["playVideo"]); //当点击某个部件时发送给父组件以禁用swiper（不能滚动到下一页或上一页）

//控制视频播放展示页的状态
let isPlayVideo = ref(false);
let videoDisplay = ref("none");
let videoOpacity = ref(0);

//窗口宽度和高度
let w2 = ref(0);
let h2 = ref(0);
const limitWidth = ref("");

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

//各部件的自适应变量
let rNewCharacter = ref("");
let wNewCharacter = ref("");
let tNewCharacter = ref("");
let hNewCharacterDetal = ref("");
let lNewCharacterDetal = ref("");
let wNewCharacterDetal = ref("");
let wVideo = ref("");
let hVideo = ref("");
let lVideo = ref("");

//角色视频链接
let characterVideos = ref([
    {
        id: 0,
        name: "提纳里",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2022/08/23/2a98cd5057e4330b3130591df0ce60b5_2023614773460852835.mp4",
    },
    {
        id: 1,
        name: "钟离",
        videoURL:
            "https://uploadstatic.mihoyo.com/hk4e/upload/officialsites/202011/%E9%92%9F%E7%A6%BB%E8%A7%92%E8%89%B2%E6%BC%94%E7%A4%BA_1606702228_4352.mp4",
    },
    {
        id: 2,
        name: "柯莱",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2022/08/22/1ae2e5f564f6c2c57cb7c494ce9f2440_1076195285047353656.mp4",
    },
    {
        id: 3,
        name: "甘雨",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2021/01/11/0ede4299ba691f5c011cd1f4a1d0b4fa_4557158493605335930.mp4",
    },
    {
        id: 4,
        name: "心海",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2021/09/19/6ad98a93af5a4caf2c8eddfd8bda51d7_3890877391063843086.mp4",
    },
    {
        id: 5,
        name: "多莉",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2022/09/02/47bd22e2f4b5bde2564a16abc8ba98cc_3449146653470709192.mp4",
    },
]);
let videoURL = ref("");

onMounted(() => {
    page2ZSY();
    window.addEventListener("resize", page2ZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", page2ZSY);
});

//自适应函数
const page2ZSY = () => {
    w2.value = document.documentElement.clientWidth;
    h2.value = document.documentElement.clientHeight;
    if (w2.value / h2.value >= proportion) {
        limitWidth.value = "100vw";
        //右上全新大世界
        rNewCharacter.value = 0.145 * w2.value + "px";
        wNewCharacter.value = 0.065 * w2.value + "px";
        tNewCharacter.value = 0 - (w2.value / proportion - h2.value) / 2 + "px";
        //中间板块
        hNewCharacterDetal.value = (0.82 * w2.value) / proportion + "px";
        wNewCharacterDetal.value = 0.55 * w2.value + "px";
        lNewCharacterDetal.value = 0.235 * w2.value + "px";
        //角色视频
        wVideo.value = 0.4 * w2.value + "px";
        hVideo.value = 0.24 * w2.value + "px";
        lVideo.value = 0.5 * w2.value + "px";
    } else {
        //右上全新大世界
        rNewCharacter.value =
            0.145 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        wNewCharacter.value = 0.065 * h2.value * proportion + "px";
        tNewCharacter.value = 0 + "px";
        //中间板块
        hNewCharacterDetal.value = 0.82 * h2.value + "px";
        wNewCharacterDetal.value = 0.55 * h2.value * proportion + "px";
        lNewCharacterDetal.value =
            0.235 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        //角色视频
        wVideo.value = 0.4 * h2.value * proportion + "px";
        hVideo.value = 0.24 * h2.value * proportion + "px";
        lVideo.value = 0.5 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        //限制区域
        if (w2.value / h2.value <= wLimitProportion) {
            limitWidth.value = h2.value * wLimitProportion + "px";
            rNewCharacter.value =
                0.145 * h2.value * proportion -
                (h2.value * proportion + h2.value * wLimitProportion - 2 * w2.value) / 2 +
                "px";
            lNewCharacterDetal.value =
                0.235 * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
            lVideo.value =
                0.5 * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
        } else {
            limitWidth.value = "100vw";
        }
    }
};

// 点击时获取对应的id并播放对应的角色视频
const videoId = (value) => {
    isPlayVideo.value = true;
    emit("playVideo", isPlayVideo.value);
    switch (value) {
        case 0:
            videoURL.value = characterVideos.value[0].videoURL;
            break;
        case 1:
            videoURL.value = characterVideos.value[1].videoURL;
            break;
        case 2:
            videoURL.value = characterVideos.value[2].videoURL;
            break;
        case 3:
            videoURL.value = characterVideos.value[3].videoURL;
            break;
        case 4:
            videoURL.value = characterVideos.value[4].videoURL;
            break;
        case 5:
            videoURL.value = characterVideos.value[5].videoURL;
            break;
        default:
            break;
    }
    videoOpen();
    // else isPlayVideo.value = false
    console.log("收到id：", value);
};

// 视频展示页的打开与关闭
const videoOpen = () => {
    if (isPlayVideo.value) {
        videoDisplay.value = "block";
        setTimeout(() => {
            videoOpacity.value = 1;
        }, 10);
    }
};

const videoClose = () => {
    videoOpacity.value = 0;
    setTimeout(() => {
        videoDisplay.value = "none";
        isPlayVideo.value = false;
        emit("playVideo", isPlayVideo.value);
    }, 300);
};
</script>

<template>
    <div class="page2">
        <!-- 视频展示页 -->
        <div
            class="videoPlay"
            @click="videoClose"
            :style="{ display: videoDisplay, opacity: videoOpacity }"
        >
            <video
                :style="{ width: wVideo, height: hVideo, left: lVideo }"
                v-if="isPlayVideo"
                autoplay="autoplay"
                controls="controls"
                preload="auto"
                class="videoPlayDetal"
                @click.stop=""
                :src="videoURL"
            ></video>
        </div>

        <div class="wlimit" :style="{ width: limitWidth }">
            <!-- 右上角“角色情报”书签 -->
            <img
                class="newCharacter"
                :style="{ top: tNewCharacter, right: rNewCharacter, width: wNewCharacter }"
                src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/6c0baa4a21b9b847d1c0dbc7f8960106_2590357259640058613.png"
                draggable="false"
                alt=""
            />
            <!-- 中间内容 -->
            <div
                class="newCharacterDetal"
                :style="{
                    width: wNewCharacterDetal,
                    left: lNewCharacterDetal,
                }"
            >
                <page3Swiper @videoId="videoId"></page3Swiper>
            </div>
        </div>
    </div>
</template>

<style scoped>
.page2 {
    position: relative;
    width: 100%;
    height: 100%;
}

.page2 .wlimit {
    height: 100%;
    background: url("https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/9a9697dd922e88d9748da1333d003e4a_5244397101809426875.png")
        center center no-repeat;
    background-size: cover;
}

.page2 .wlimit .newCharacter {
    position: absolute;
    right: 0;
    top: 0;
    width: 72px;
}

.page2 .wlimit .newCharacterDetal {
    position: relative;
    /* top: 50%; */
    height: 100%;
    overflow: hidden;
    /* transform: translate(0, -50%); */
    /* background-color: rgba(0, 0, 0, 0.3); */
    /* overflow: hidden; */
}

.videoPlay {
    display: none;
    opacity: 0;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    z-index: 10;
    transition: 0.3s linear;
}

.videoPlayDetal {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 500px;
    height: 300px;
    transform: translate(-50%, -50%);
    /* background-color: rgba(255, 0, 0, 0.2); */
}
</style>
