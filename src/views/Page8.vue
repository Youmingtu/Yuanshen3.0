<script setup>
import { onUnmounted, onMounted, ref } from "vue";

let h = ref(0);
let w = ref(0);
let hLogo = ref("");
let lLogo = ref("");
let tLogo = ref("");

const limitWidth = ref("");
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

onMounted(() => {
    p8logoZSY();
    window.addEventListener("resize", p8logoZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", p8logoZSY);
});

const p8logoZSY = () => {
    w.value = document.documentElement.clientWidth;
    h.value = document.documentElement.clientHeight;
    if (w.value / h.value >= proportion) {
        limitWidth.value = "100vw";
        hLogo.value = ((5 / 54) * w.value) / proportion + "px";
        lLogo.value = 0.5 * w.value + "px";
    } else {
        hLogo.value = (5 / 54) * h.value + "px";
        lLogo.value = 0.5 * h.value * proportion - (h.value * proportion - w.value) / 2 + "px";
        //限制区域
        if (w.value / h.value <= wLimitProportion) {
            lLogo.value =
                0.5 * h.value * proportion -
                (h.value * proportion - h.value * wLimitProportion) / 2 +
                "px";
        } else {
            limitWidth.value = "100vw";
        }
    }
};
</script>

<template>
    <div class="fotterLogo" :style="{ height: hLogo, left: lLogo, top: tLogo }">
        <img
            class="logo1"
            src="https://webstatic.mihoyo.com/upload/event/2021/03/01/ed39ba943da56c88dec2b18f4bfb0a32_2315648370550091638.png"
            alt=""
            draggable="false"
        />
        <span class="logoI"></span>
        <img
            class="logo2"
            src="https://webstatic.mihoyo.com/bh3/upload/officialsites/201908/ys_1565764084_7084.png"
            alt=""
            draggable="false"
        />
    </div>
</template>

<style scoped>
.fotterLogo {
    position: absolute;
    left: 50%;
    top: 50%;
    width: auto;
    height: 80px;
    display: flex;
    align-items: center;
    transform: translate(-60%, -50%);
    /* background-color: rgba(255, 0, 0, 0.2); */
}

.logo1 {
    height: 100%;
    transform: scale(0.5);
}
.logo2 {
    height: 100%;
}
.logoI {
    height: 35%;
    margin-right: 50px;
    border-left: 1px solid white;
}
</style>
