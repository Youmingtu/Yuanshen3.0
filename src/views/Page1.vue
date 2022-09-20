<script setup>
import { onBeforeMount, onMounted, onUnmounted, onUpdated, ref, watchEffect } from "vue";

const emit = defineEmits(["MWControl"]); //当点击某个部件时发送给父组件以禁用swiper（不能滚动到下一页或上一页）

//以下是相关部件的自适应变量
let w = ref(0);
let h = ref(0);
let l = ref(0);
let mw = ref(true);
let videoShow = ref(false);
let wlimit = ref(0);
let hideEffect = ref(0);
let hide = ref("none");
let CDHideEffect = ref(0);
let CDHide = ref("none");
let videoHideEffect = ref(0);
let videoHide = ref("none");
let limitWidth = ref("");
let divWidth = ref("");
let divHeight = ref("");
let divleft = ref("");
let enterRight = ref("");
let enterWidth = ref("");
let ageDetalTop = ref("");
let videoDetalTop = ref("");
let ageDetalLeft = ref("");
let ageWidth = ref("");
let CDDetalTop = ref("");
let CDDetalLeft = ref("");
let DownloadTop = ref("");
let DownloadLeft = ref("");
let DownloadWidth = ref("");
let playerTop = ref("");
let playerLeft = ref("");
let playerWidth = ref("");
let ageDetalWidth = ref("");
let CDDetalWidth = ref("");
let QRDetalWidth = ref("");

//某些按钮的背景图（进入官网、播放器、PC下载、云下载）
let enterBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/b155f2ee547b4cc0df22d2ad540da187_6417227238309055251.png",
);
let playerBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/9506bf6218c35e652d6ad8ba10f7b69f_8652210465750230921.png",
);
let pcdBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/1877e5c169de6d397255c0fbb72c58bb_1051279672834509138.png",
);
let cdBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/773223ba04cbd1649cd171a9eb31daac_2725468403831075314.png",
);

//需要用到的标准窗口比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

onMounted(() => {
    selfAdaption();
    console.log("当前宽度:", w.value, " ", "当前高度:", h.value);
    //窗口监听事件，并执行回调函数（自适应函数）
    window.addEventListener("resize", selfAdaption);
});

onUnmounted(() => {
    window.removeEventListener("resize", selfAdaption);
});

//自适应方法（第一次写有点乱，后面的page会清楚许多）
const selfAdaption = () => {
    w.value = document.documentElement.clientWidth;
    h.value = document.documentElement.clientHeight;
    if (w.value / h.value >= proportion) {
        divWidth.value = w.value / 11 + "px";
        divHeight.value = (w.value * (1 / proportion)) / 6.7 + "px";
        //12+
        ageWidth.value = w.value / 14 + "px";
        //进入官网
        enterWidth.value = w.value / 12 + "px";
        //12+信息
        ageDetalWidth.value = w.value / 3 + "px";
        //云下载信息
        CDDetalWidth.value = w.value / 6 + "px";
        //二维码信息
        QRDetalWidth.value = w.value / 10 + "px";
        //download
        DownloadWidth.value = w.value / 13 + "px";
        //player
        playerWidth.value = w.value / 25 + "px";

        divleft.value = 0.125 * w.value + "px";
        enterRight.value = 0.108 * w.value + "px";

        ageDetalTop.value =
            (0.2 * w.value) / proportion - (w.value / proportion - h.value) / 2 + "px";
        videoDetalTop.value =
            (0.3 * w.value) / proportion - (w.value / proportion - h.value) / 2 + "px";
        CDDetalTop.value =
            (0.278 * w.value) / proportion - (w.value / proportion - h.value) / 2 + "px";
        ageDetalLeft.value = 0.33 * w.value + "px";
        CDDetalLeft.value = 0.4142 * w.value + "px";

        playerTop.value =
            (0.42 * w.value) / proportion - (w.value / proportion - h.value) / 2 + "px";
        playerLeft.value = 0.4 * w.value + "px";

        DownloadTop.value =
            (0.743 * w.value) / proportion - (w.value / proportion - h.value) / 2 + "px";
        DownloadLeft.value = 0.4417 * w.value + "px";
    } else {
        divWidth.value = (h.value * proportion) / 11 + "px";
        divHeight.value = h.value / 6.7 + "px";
        //12+
        ageWidth.value = (h.value * proportion) / 14 + "px";
        //进入官网
        enterWidth.value = (h.value * proportion) / 12 + "px";
        //12+信息
        ageDetalWidth.value = (h.value * proportion) / 3 + "px";
        //云下载信息
        CDDetalWidth.value = (h.value * proportion) / 6 + "px";
        //二维码信息
        QRDetalWidth.value = (h.value * proportion) / 10 + "px";
        //download
        DownloadWidth.value = (h.value * proportion) / 13 + "px";
        //player
        playerWidth.value = (h.value * proportion) / 25 + "px";

        divleft.value =
            0.125 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) + "px";
        enterRight.value =
            0.108 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) + "px";

        ageDetalTop.value = 0.2 * h.value + "px";
        videoDetalTop.value = 0.3 * h.value + "px";
        CDDetalTop.value = 0.278 * h.value + "px";
        ageDetalLeft.value =
            0.33 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) + "px";
        CDDetalLeft.value =
            0.4142 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) + "px";

        playerTop.value = 0.42 * h.value + "px";
        playerLeft.value =
            0.4 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) + "px";

        DownloadTop.value = 0.743 * h.value + "px";
        DownloadLeft.value =
            0.4417 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) + "px";

        if (0.125 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) <= 0)
            divleft.value = "0px";
        if (0.108 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) <= 10)
            enterRight.value = "10px";
        if (
            0.33 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) <=
            0.1508 * h.value * proportion
        )
            ageDetalLeft.value = 0.1508 * h.value * proportion + "px";
        if (
            0.4142 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) <=
            0.2353 * h.value * proportion
        )
            CDDetalLeft.value = 0.2353 * h.value * proportion + "px";
        if (
            0.4 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) <=
            0.22 * h.value * proportion
        )
            playerLeft.value = 0.22 * h.value * proportion + "px";

        if (
            0.4417 * h.value * proportion - ((h.value * proportion) / 2 - w.value / 2) <=
            0.2634 * h.value * proportion
        )
            DownloadLeft.value = 0.2634 * h.value * proportion + "px";

        if (w.value / h.value <= wLimitProportion) {
            limitWidth.value = h.value * wLimitProportion + "px";
            enterRight.value = 10 - (h.value * wLimitProportion - w.value) + "px";
        } else {
            limitWidth.value = "100vw";
        }
    }
};

//点击左下角“12+”时
const ageClickIn = () => {
    hide.value = "block";
    mw.value = false;
    emit("MWControl", mw.value);
    setTimeout(() => {
        hideEffect.value = 1;
    }, 10);
};

const ageClickOut = () => {
    hideEffect.value = 0;
    setTimeout(() => {
        hide.value = "none";
        mw.value = true;
        emit("MWControl", mw.value);
    }, 300);
};

//点击底下“云下载”时
const CDClickIn = () => {
    CDHide.value = "block";
    mw.value = false;
    emit("MWControl", mw.value);
    setTimeout(() => {
        CDHideEffect.value = 1;
    }, 10);
};

const CDClickOut = () => {
    CDHideEffect.value = 0;
    setTimeout(() => {
        CDHide.value = "none";
        mw.value = true;
        emit("MWControl", mw.value);
    }, 300);
};

//点击中间“播放视频按钮”时
const videoClickIn = () => {
    videoShow.value = true;
    videoHide.value = "block";
    mw.value = false;
    emit("MWControl", mw.value);
    setTimeout(() => {
        videoHideEffect.value = 1;
    }, 10);
};

const videoClickOut = () => {
    videoHideEffect.value = 0;
    setTimeout(() => {
        videoHide.value = "none";
        videoShow.value = false;
        mw.value = true;
        emit("MWControl", mw.value);
    }, 300);
};

//点击右上角“进入官网”时
const enterWebHoverEnter = () => {
    enterBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/1bbb06de09017ac93e7254d0b81ef2c3_5835590094678703406.png";
};

const enterWebHoverleave = () => {
    enterBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/b155f2ee547b4cc0df22d2ad540da187_6417227238309055251.png";
};

//中间“视频播放按钮”的mouseover和mouseleave改变按钮图片
const playerHI = () => {
    playerBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/ee87694a495d8ef8516ba66c163248f4_1018905587756356306.png";
};

const playerHO = () => {
    playerBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/9506bf6218c35e652d6ad8ba10f7b69f_8652210465750230921.png";
};

//底下“PC下载”的mouseover和mouseleave改变按钮图片
const PCDHI = () => {
    pcdBg.value =
        "data:image/webp;base64,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";
};

const PCDHO = () => {
    pcdBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/1877e5c169de6d397255c0fbb72c58bb_1051279672834509138.png";
};

//底下“云下载”的mouseover和mouseleave改变按钮图片
const CDHI = () => {
    cdBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/8c0837403e1c9d8329e6240415c39ac7_3262756500097035108.png";
};

const CDHO = () => {
    cdBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/773223ba04cbd1649cd171a9eb31daac_2725468403831075314.png";
};

const PCD = () => {
    alert("暂无下载\n官网是一点就会下载:>_<:");
};
</script>

<template>
    <div class="page1">
        <div class="wlimit" :style="{ width: limitWidth }">
            <!-- 左上角logo -->
            <a id="logo" href="https://ys.mihoyo.com/main/" target="_blank">
                <img
                    class="logo"
                    :style="{ width: divWidth, height: divHeight, left: divleft }"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/b80b3a885bc5367326d90b66934c2020_4696293442908026759.png"
                    alt=""
                    @dragstart.prevent
                />
            </a>
            <!-- 左下角12+ -->
            <div class="ageTips" :style="{ left: divleft }">
                <img
                    :style="{ width: ageWidth }"
                    @click="ageClickIn"
                    src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/e85de8f98080d4e8412fe93244e93ae1_8726960985593761961.png"
                    alt=""
                    @dragstart.prevent
                />
            </div>
            <!-- 12+信息展示页 -->
            <div
                class="ageDetal"
                @click.stop="ageClickOut"
                :style="{ opacity: hideEffect, display: hide }"
            >
                <img
                    @click.stop=""
                    class="ageDetal-bg"
                    :style="{ width: ageDetalWidth, top: ageDetalTop, left: ageDetalLeft }"
                    src="https://webstatic.mihoyo.com/upload/puzzle/2021/11/19/11e7a079077d5523f87ec34bf65a71fa_7483736760903107786.jpg"
                    alt=""
                    @dragstart.prevent
                />
            </div>
            <!-- 底下下载部分 -->
            <div
                class="Download"
                :style="{ width: DownloadWidth, top: DownloadTop, left: DownloadLeft }"
            >
                <!-- PC下载按钮 -->
                <div>
                    <img
                        class="PCDownload"
                        @click="PCD"
                        @mouseover="PCDHI"
                        @mouseleave="PCDHO"
                        :src="pcdBg"
                        alt=""
                        @dragstart.prevent
                    />
                </div>
                <!-- 云下载按钮 -->
                <div>
                    <img
                        class="CloudDownload"
                        @click.stop="CDClickIn"
                        @mouseover="CDHI"
                        @mouseleave="CDHO"
                        :src="cdBg"
                        alt=""
                        @dragstart.prevent
                    />
                </div>
            </div>
            <!-- 云下载展示页 -->
            <div
                class="CDDetal"
                @click.stop="CDClickOut"
                :style="{ opacity: CDHideEffect, display: CDHide }"
            >
                <div
                    @click.stop=""
                    class="CDDetal-bg"
                    :style="{ top: CDDetalTop, left: CDDetalLeft }"
                >
                    <img
                        :style="{ width: CDDetalWidth }"
                        src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/bae38f959920cca49684d232bf20e007_6795612514447984716.png"
                        alt=""
                        @dragstart.prevent
                    />
                    <img
                        class="CDDetal-QR"
                        :style="{ width: QRDetalWidth }"
                        src="https://webstatic.mihoyo.com/upload/static-resource/2022/02/11/1b60424c568b252cc15154126573aa0b_7200878359902840345.png"
                        alt=""
                        @dragstart.prevent
                    />
                </div>
            </div>
            <!-- 右上角进入官网 -->
            <div class="enterWeb" :style="{ right: enterRight }">
                <a href="https://ys.mihoyo.com/main/" target="_blank">
                    <img
                        @mouseover="enterWebHoverEnter"
                        @mouseleave="enterWebHoverleave"
                        :style="{ width: enterWidth }"
                        :src="enterBg"
                        alt=""
                        @dragstart.prevent
                    />
                </a>
            </div>
            <!-- 中间视频播放按钮 -->
            <div class="player" :style="{ width: playerWidth, top: playerTop, left: playerLeft }">
                <img
                    @click="videoClickIn"
                    @mouseover="playerHI"
                    @mouseleave="playerHO"
                    class="player-bg"
                    :src="playerBg"
                    alt=""
                    @dragstart.prevent
                />
            </div>
            <!-- 视频展示页 -->
            <div
                class="videoDetal"
                @click.stop="videoClickOut"
                :style="{ opacity: videoHideEffect, display: videoHide }"
            >
                <video
                    v-if="videoShow"
                    autoplay="autoplay"
                    controls="controls"
                    preload="auto"
                    @click.stop=""
                    class="videoDetal-bg"
                    :style="{ width: ageDetalWidth, top: videoDetalTop, left: ageDetalLeft }"
                    src="https://webstatic.mihoyo.com/upload/static-resource/2022/08/13/66ab78529ed42e68e8a30fc5207d85b6_290673737464782301.mp4"
                ></video>
            </div>
        </div>
    </div>
</template>

<style scoped>
.page1 {
    position: relative;
    width: 100%;
    height: 100%;
}

.page1 .wlimit {
    height: 100%;
    background: url("https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/f8f8b39b79082490dc7a51cb58c033a4_1886787457893712308.jpg")
        center center no-repeat;
    background-size: cover;
}
/* 
.page1 .wlimit .mousewheel {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: red;
    opacity: 0;
    z-index: 0;
} */

.page1 .logo {
    position: absolute;
    top: 12px;
    object-fit: contain;
}

.page1 .ageTips {
    position: absolute;
    bottom: 5px;
    transform: scale(0.6);
    /* object-fit: cover; */
}

.page1 .ageDetal {
    position: relative;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    align-content: center;
    vertical-align: center;
    transition: all 0.3s;
    z-index: 10;
}

.page1 .ageDetal .ageDetal-bg {
    position: absolute;
    object-fit: contain;
}

.page1 .CDDetal {
    position: relative;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    align-content: center;
    vertical-align: center;
    transition: all 0.3s;
    z-index: 10;
}

.page1 .CDDetal .CDDetal-bg {
    position: absolute;
    object-fit: contain;
}

.page1 .CDDetal .CDDetal-QR {
    position: absolute;
    top: 25%;
    left: 18%;
    object-fit: contain;
}

.page1 .videoDetal {
    position: relative;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    align-content: center;
    vertical-align: center;
    transition: all 0.3s;
    z-index: 10;
}

.page1 .videoDetal .videoDetal-bg {
    position: absolute;
    object-fit: contain;
}

.page1 .Download {
    position: absolute;
}

.page1 .Download .PCDownload {
    width: 100%;
}

.page1 .Download .CloudDownload {
    margin-top: 5px;
    width: 100%;
}

.page1 .enterWeb {
    position: absolute;
    display: inline-block;
    top: 30px;
}

.page1 .player {
    position: absolute;
}

.page1 .player .player-bg {
    width: 100%;
}
</style>
