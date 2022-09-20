<script setup>
import { onUnmounted, onMounted, ref, watch } from "vue";

import page2Swiper from "../components/Page2Swiper.vue";

// 获取窗口宽度高度
let w2 = ref(0);
let h2 = ref(0);
const limitWidth = ref("");

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

//以下是相关部件的自适应变量
let rNewWorld = ref("");
let wNewWorld = ref("");
let tNewWorld = ref("");
let hNewWorldDetal = ref("");
let lNewWorldDetal = ref("");
let wNewWorldDetal = ref("");

onMounted(() => {
    page2ZSY();
    //窗口监听并执行自适应函数
    window.addEventListener("resize", page2ZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", page2ZSY);
});

// 自适应函数
const page2ZSY = () => {
    w2.value = document.documentElement.clientWidth;
    h2.value = document.documentElement.clientHeight;
    if (w2.value / h2.value >= proportion) {
        limitWidth.value = "100vw";
        //右上全新大世界
        rNewWorld.value = 0.145 * w2.value + "px";
        wNewWorld.value = 0.065 * w2.value + "px";
        tNewWorld.value = 0 - (w2.value / proportion - h2.value) / 2 + "px";
        //中间板块
        hNewWorldDetal.value = (0.789 * w2.value) / proportion + "px";
        wNewWorldDetal.value = 0.52 * w2.value + "px";
        lNewWorldDetal.value = 0.263 * w2.value + "px";
    } else {
        //右上全新大世界
        rNewWorld.value =
            0.145 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        wNewWorld.value = 0.065 * h2.value * proportion + "px";
        tNewWorld.value = 0 + "px";
        //中间板块
        hNewWorldDetal.value = 0.789 * h2.value + "px";
        wNewWorldDetal.value = 0.52 * h2.value * proportion + "px";
        lNewWorldDetal.value =
            0.263 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        //限制区域
        if (w2.value / h2.value <= wLimitProportion) {
            limitWidth.value = h2.value * wLimitProportion + "px";
            rNewWorld.value =
                0.145 * h2.value * proportion -
                (h2.value * proportion + h2.value * wLimitProportion - 2 * w2.value) / 2 +
                "px";
            lNewWorldDetal.value =
                0.263 * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
        } else {
            limitWidth.value = "100vw";
        }
    }
};
</script>

<template>
    <div class="page2">
        <div class="wlimit" :style="{ width: limitWidth }">
            <img
                class="newWorld"
                :style="{ top: tNewWorld, right: rNewWorld, width: wNewWorld }"
                src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/23/5c1b9b612986872b4b8104b5572d187a_8441111753714725480.png"
                draggable="false"
                alt=""
            />
            <div
                class="newWorldDetal"
                :style="{ width: wNewWorldDetal, left: lNewWorldDetal, height: hNewWorldDetal }"
            >
                <page2Swiper></page2Swiper>
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
    background: url("https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/6371e3af0f8186f9791292b64c6af1e1_1135136736991747641.jpg")
        center center no-repeat;
    background-size: cover;
}

.page2 .wlimit .newWorld {
    position: absolute;
    right: 0;
    top: 0;
    width: 72px;
}

.page2 .wlimit .newWorldDetal {
    position: absolute;
    top: 50%;
    width: 520px;
    height: 375px;
    transform: translate(0, -50%);
    /* overflow: hidden; */
}
</style>
