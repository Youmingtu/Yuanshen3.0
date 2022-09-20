<script setup>
import { onUnmounted, onMounted, ref, watch } from "vue";

import page7Swiper from "../components/Page7Swiper.vue";
import p7FooterIcon from "../components/P7FooterIcon.vue";

const emit = defineEmits(["canScroll"]); //当点击某个部件时发送给父组件以禁用swiper（不能滚动到下一页或上一页）
let canHideVX = ref(false); //是否可以点击底下"微信号"

//窗口的宽度和高度
let w2 = ref(0);
let h2 = ref(0);
const limitWidth = ref("");

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

// 各部件的自适应变量
let rNewWorld = ref("");
let wNewWorld = ref("");
let tNewWorld = ref("");
let hNewWorldDetal = ref("");
let lNewWorldDetal = ref("");
let wNewWorldDetal = ref("");
let hFooter = ref("");
let wFooter = ref("");
let bFooter = ref("");
let lvxDetal = ref("");
let tvxDetal = ref("");
let wvxDetal = ref("");
let fsvxName = ref("");
let lvxCode = ref("");
let tvxCode = ref("");
let wvxCode = ref("");

//底下"微信号"展示页的状态
let displayVX = ref("none");
let opacityVX = ref(0);

onMounted(() => {
    page7ZSY();
    window.addEventListener("resize", page7ZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", page7ZSY);
});

//自适应函数
const page7ZSY = () => {
    w2.value = document.documentElement.clientWidth;
    h2.value = document.documentElement.clientHeight;
    if (w2.value / h2.value >= proportion) {
        limitWidth.value = "100vw";
        //右上游戏特色
        rNewWorld.value = 0.145 * w2.value + "px";
        wNewWorld.value = 0.065 * w2.value + "px";
        tNewWorld.value = 0 - (w2.value / proportion - h2.value) / 2 + "px";
        //中间板块
        hNewWorldDetal.value = (0.54 * w2.value) / proportion + "px";
        wNewWorldDetal.value = 0.45 * w2.value + "px";
        lNewWorldDetal.value = 0 + "px";
        //底下图标区域
        hFooter.value = ((65 / 540) * w2.value) / proportion + "px";
        bFooter.value =
            ((45 / 540) * w2.value) / proportion - (w2.value / proportion - h2.value) / 2 + "px";
        //微信公众号
        lvxDetal.value = (465 / 1250) * w2.value + "px";
        tvxDetal.value =
            ((11 / 54) * w2.value) / proportion - (w2.value / proportion - h2.value) / 2 + "px";
        wvxDetal.value = (325 / 1250) * w2.value + "px";
        //微信公众号 - text
        fsvxName.value = ((14 / 540) * w2) / proportion + "px";
        //微信公众号 - Code
        lvxCode.value = (155 / 465) * (325 / 1250) * w2.value + "px";
        tvxCode.value = 0.5 * (325 / 1250) * w2.value + "px";
        wvxCode.value = 0.33 * (325 / 1250) * w2.value + "px";
    } else {
        //右上游戏特色
        rNewWorld.value =
            0.145 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        wNewWorld.value = 0.065 * h2.value * proportion + "px";
        tNewWorld.value = 0 + "px";
        //中间板块
        hNewWorldDetal.value = 0.54 * h2.value + "px";
        wNewWorldDetal.value = 0.45 * h2.value * proportion + "px";
        lNewWorldDetal.value = 0 + "px";
        //底下图标区域
        hFooter.value = (65 / 540) * h2.value + "px";
        bFooter.value = (45 / 540) * h2.value + "px";
        if (w2.value <= (35 / 1250) * h2.value * proportion) {
            wFooter.value = (35 / 1250) * h2.value * proportion + "px";
        }
        //微信公众号
        lvxDetal.value =
            (465 / 1250) * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        tvxDetal.value = (11 / 54) * h2.value + "px";
        wvxDetal.value = (325 / 1250) * h2.value * proportion + "px";
        //微信公众号 - text
        fsvxName.value = (14 / 540) * h2.value + "px";
        //微信公众号 - Code
        lvxCode.value = (155 / 465) * (325 / 1250) * h2.value * proportion + "px";
        tvxCode.value = 0.5 * (325 / 1250) * h2.value * proportion + "px";
        wvxCode.value = 0.33 * (325 / 1250) * h2.value * proportion + "px";
        //限制区域
        if (w2.value / h2.value <= wLimitProportion) {
            limitWidth.value = h2.value * wLimitProportion + "px";
            rNewWorld.value =
                0.145 * h2.value * proportion -
                (h2.value * proportion + h2.value * wLimitProportion - 2 * w2.value) / 2 +
                "px";
            // lNewWorldDetal.value =
            //     0 * h2.value * proportion -
            //     (h2.value * proportion - h2.value * wLimitProportion) / 2 +
            //     "px";
            lNewWorldDetal.value = 0 + (h2.value * wLimitProportion - w2.value) / 2 + "px";
            lvxDetal.value =
                (465 / 1250) * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
        } else {
            limitWidth.value = "100vw";
        }
    }
};

//"微信号"展示页的显示和隐藏
const vxHide = () => {
    if (canHideVX.value) {
        opacityVX.value = 0;
        setTimeout(() => {
            displayVX.value = "none";
            emit("canScroll", true);
        }, 300);
    }
};

const vxOpen = (val) => {
    emit("canScroll", false);
    displayVX.value = "block";
    setTimeout(() => {
        opacityVX.value = 1;
    }, 10);
    setTimeout(() => {
        canHideVX.value = true;
    }, 300);
    console.log("收到:11111", val);
};
</script>

<template>
    <div class="page7">
        <div class="wlimit" :style="{ width: limitWidth }">
            <!-- 右上角“游戏特色”书签 -->
            <img
                class="newWorld"
                :style="{ top: tNewWorld, right: rNewWorld, width: wNewWorld }"
                src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/addb102e19e1d810b200ba35c73a22b9_34975060921510792.png"
                draggable="false"
                alt=""
            />
            <!-- 中间部分 -->
            <div class="newWorldDetal" :style="{ height: hNewWorldDetal, left: lNewWorldDetal }">
                <page7Swiper></page7Swiper>
            </div>
            <!-- 底下的四个图标部分 -->
            <div class="footer" :style="{ width: wFooter, height: hFooter, bottom: bFooter }">
                <p7FooterIcon @vxOpen="vxOpen"></p7FooterIcon>
            </div>
            <!-- "微信号"展示页 -->
            <div class="vx" :style="{ display: displayVX, opacity: opacityVX }" @click="vxHide">
                <div
                    class="vxDetal"
                    :style="{ left: lvxDetal, top: tvxDetal, width: wvxDetal }"
                    @click.stop=""
                >
                    <img
                        class="vxHead"
                        src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/07/11/8d4e533df874da23cf6e7f451be57dca_944877516421628953.png"
                        alt=""
                        draggable="false"
                    />
                    <img
                        class="vxCode"
                        :style="{ left: lvxCode, top: tvxCode, width: wvxCode }"
                        src="https://webstatic.mihoyo.com/upload/puzzle/2021/11/19/6a8f260d01b18a97874a70611fac7f84_1193977791953494844.png"
                        alt=""
                        draggable="false"
                    />
                    <div class="vxName" :style="{ fontSize: fsvxName }">- 官方微信号 -</div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.page7 {
    position: relative;
    width: 100%;
    height: 100%;
}

/* 每一页的背景图 */
.page7 .wlimit {
    height: 100%;
    background: url("https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/672290c5f62b10ba1ce97807a2ea5859_7217031592658053774.jpg")
        center center no-repeat;
    background-size: cover;
}

.page7 .wlimit .newWorld {
    position: absolute;
    right: 0;
    top: 0;
    width: 72px;
}

.page7 .wlimit .newWorldDetal {
    position: absolute;
    top: 42%;
    /* right: 40px; */
    width: 100%;
    height: 540px;
    transform: translate(0, -50%);
    /* overflow: hidden; */
}

.footer {
    position: absolute;
    left: 0;
    bottom: 45px;
    width: 100%;
    height: 65px;
    /* background-color: rgba(0, 0, 0, 0.2); */
}

.vx {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    transition: 0.3s;
}

.vxDetal {
    position: absolute;
    left: 465px;
    top: 110px;
    width: 325px;
}

.vxHead {
    width: 100%;
}

.vxCode {
    position: absolute;
    left: 110px;
    top: 160px;
    width: 107px;
}
.vxName {
    position: relative;
    top: -30px;
    left: 0;
    height: 14px;
    font-size: 14px;
    color: white;
    text-align: center;
}
</style>
