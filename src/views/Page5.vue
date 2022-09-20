<script setup>
import { onUnmounted, onMounted, ref, watch } from "vue";

import Page5Swiper from "../components/Page5Swiper.vue";

//窗口的宽度与高度
let w2 = ref(0);
let h2 = ref(0);
const limitWidth = ref("");

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

//各部件的自适应变量
let lNewMechanism = ref("");
let wNewMechanism = ref("");
let tNewMechanism = ref("");
let hNewMechanismDetal = ref("");
let lNewMechanismDetal = ref("");
let wNewMechanismDetal = ref("");

onMounted(() => {
    page5ZSY();
    window.addEventListener("resize", page5ZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", page5ZSY);
});

// 自适应函数
const page5ZSY = () => {
    w2.value = document.documentElement.clientWidth;
    h2.value = document.documentElement.clientHeight;
    if (w2.value / h2.value >= proportion) {
        limitWidth.value = "100vw";
        //左上全新大世界
        lNewMechanism.value = 0.145 * w2.value + "px";
        wNewMechanism.value = 0.065 * w2.value + "px";
        tNewMechanism.value = 0 - (w2.value / proportion - h2.value) / 2 + "px";
        //中间板块
        hNewMechanismDetal.value = (0.789 * w2.value) / proportion + "px";
        wNewMechanismDetal.value = 0.52 * w2.value + "px";
        lNewMechanismDetal.value = 0.263 * w2.value + "px";
    } else {
        //左上全新大世界
        lNewMechanism.value =
            0.145 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        wNewMechanism.value = 0.065 * h2.value * proportion + "px";
        tNewMechanism.value = 0 + "px";
        //中间板块
        hNewMechanismDetal.value = 0.789 * h2.value + "px";
        wNewMechanismDetal.value = 0.52 * h2.value * proportion + "px";
        lNewMechanismDetal.value =
            0.263 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        //限制区域
        if (w2.value / h2.value <= wLimitProportion) {
            limitWidth.value = h2.value * wLimitProportion + "px";
            lNewMechanism.value =
                0.145 * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
            lNewMechanismDetal.value =
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
    <div class="page5">
        <div class="wlimit" :style="{ width: limitWidth }">
            <!-- 左上角"全新机制"书签 -->
            <img
                class="NewMechanism"
                :style="{ top: tNewMechanism, left: lNewMechanism, width: wNewMechanism }"
                src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/cbfe98ffb3ffb645585db21de8f52f83_3162492610851042644.png"
                draggable="false"
                alt=""
            />
            <!-- 中间部分 -->
            <div class="NewMechanismDetal">
                <Page5Swiper></Page5Swiper>
            </div>
        </div>
    </div>
</template>

<style scoped>
.page5 {
    position: relative;
    width: 100%;
    height: 100%;
}

.page5 .wlimit {
    height: 100%;
    background: url("https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/f03354a5ca390be9da0dc5d857d5edaa_5788055165160847288.jpg")
        center center no-repeat;
    background-size: cover;
}

.page5 .wlimit .NewMechanism {
    position: absolute;
    right: 0;
    top: 0;
    width: 72px;
}

.page5 .wlimit .NewMechanismDetal {
    position: absolute;
    top: 50%;
    width: 100%;
    height: 100%;
    transform: translate(0, -50%);
}
</style>
