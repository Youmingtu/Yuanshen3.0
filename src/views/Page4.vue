<script setup>
import { onUnmounted, onMounted, ref, watch } from "vue";

import Page4Swiper from "../components/Page4Swiper.vue";

//窗口宽度和高度
let w2 = ref(0);
let h2 = ref(0);
const limitWidth = ref("");

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

//各部件的自适应变量
let lNewMonster = ref("");
let wNewMonster = ref("");
let tNewMonster = ref("");
let hNewMonsterDetal = ref("");
let lNewMonsterDetal = ref("");
let wNewMonsterDetal = ref("");

onMounted(() => {
    page4ZSY();
    window.addEventListener("resize", page4ZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", page4ZSY);
});

//自适应函数
const page4ZSY = () => {
    w2.value = document.documentElement.clientWidth;
    h2.value = document.documentElement.clientHeight;
    if (w2.value / h2.value >= proportion) {
        limitWidth.value = "100vw";
        //左上全新大世界
        lNewMonster.value = 0.145 * w2.value + "px";
        wNewMonster.value = 0.065 * w2.value + "px";
        tNewMonster.value = 0 - (w2.value / proportion - h2.value) / 2 + "px";
        //中间板块
        hNewMonsterDetal.value = (0.789 * w2.value) / proportion + "px";
        wNewMonsterDetal.value = 0.52 * w2.value + "px";
        lNewMonsterDetal.value = 0.263 * w2.value + "px";
    } else {
        //左上全新大世界
        lNewMonster.value =
            0.145 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        wNewMonster.value = 0.065 * h2.value * proportion + "px";
        tNewMonster.value = 0 + "px";
        //中间板块
        hNewMonsterDetal.value = 0.789 * h2.value + "px";
        wNewMonsterDetal.value = 0.52 * h2.value * proportion + "px";
        lNewMonsterDetal.value =
            0.263 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        //限制区域
        if (w2.value / h2.value <= wLimitProportion) {
            limitWidth.value = h2.value * wLimitProportion + "px";
            lNewMonster.value =
                0.145 * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
            lNewMonsterDetal.value =
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
    <div class="page4">
        <div class="wlimit" :style="{ width: limitWidth }">
            <!-- 左上角"全新怪物"书签 -->
            <img
                class="NewMonster"
                :style="{ top: tNewMonster, left: lNewMonster, width: wNewMonster }"
                src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/897d0e11050e8b97948d639ad90a9504_1056337144196497578.png"
                draggable="false"
                alt=""
            />
            <!-- 中间部分 -->
            <div class="NewMonsterDetal">
                <page4Swiper></page4Swiper>
            </div>
        </div>
    </div>
</template>

<style scoped>
.page4 {
    position: relative;
    width: 100%;
    height: 100%;
}

.page4 .wlimit {
    height: 100%;
    background: url("https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/527d7cfa717eef6a45bdaefbec8d237b_6849912691595105870.jpg")
        center center no-repeat;
    background-size: cover;
}

.page4 .wlimit .NewMonster {
    position: absolute;
    right: 0;
    top: 0;
    width: 72px;
}

.page4 .wlimit .NewMonsterDetal {
    position: absolute;
    top: 50%;
    width: 100%;
    height: 100%;
    transform: translate(0, -50%);
}
</style>
