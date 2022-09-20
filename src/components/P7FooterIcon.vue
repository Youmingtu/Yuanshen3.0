<script setup>
import { onUnmounted, onMounted, ref } from "vue";

let footerIcon = ref(null);
const emit = defineEmits(["vxOpen"]);

// 底下图片数据
let footerIcons = ref([
    {
        id: 0,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/dc0d738971ce14760fa22302739589b7_6184953676476702286.png",
    },
    {
        id: 1,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/b1eb003f78ccfc7b8eb6fda11279a588_2042388589199221716.png",
    },
    {
        id: 2,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/85e3fb1e13e295daf89762afefe06601_5142091714308898800.png",
    },
    {
        id: 3,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/7f8e77a962599583d0678de9141791e7_5887176268253724597.png",
    },
]);
let iconLink = ref([
    { id: 0, url: "https://weibo.com/ysmihoyo" },
    { id: 2, url: "https://space.bilibili.com/401742377/" },
    { id: 3, url: "https://bbs.mihoyo.com/ys" },
]);

let h = ref(0);
let w = ref(0);
let lp7Footer = ref("");
let wp7Footer = ref("");

const limitWidth = ref("");
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

onMounted(() => {
    p7FooterZSY();
    window.addEventListener("resize", p7FooterZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", p7FooterZSY);
});

//自适应
const p7FooterZSY = () => {
    w.value = document.documentElement.clientWidth;
    h.value = document.documentElement.clientHeight;
    if (w.value / h.value >= proportion) {
        limitWidth.value = "100vw";
        footerIcon.value[0].style.marginRight = (45 / 1250) * w.value + "px";
        footerIcon.value[1].style.marginRight = (35 / 1250) * w.value + "px";
        footerIcon.value[2].style.marginRight = footerIcon.value[1].style.marginRight;
        lp7Footer.value = (45 / 125) * w.value + "px";
        wp7Footer.value = (35 / 125) * w.value + "px";
    } else {
        footerIcon.value[0].style.marginRight = (45 / 1250) * h.value * proportion + "px";
        footerIcon.value[1].style.marginRight = (35 / 1250) * h.value * proportion + "px";
        footerIcon.value[2].style.marginRight = footerIcon.value[1].style.marginRight;
        lp7Footer.value =
            (45 / 125) * h.value * proportion - (h.value * proportion - w.value) / 2 + "px";
        wp7Footer.value = (35 / 125) * h.value * proportion + "px";
        //限制区域
        if (w.value / h.value <= wLimitProportion) {
            lp7Footer.value =
                (45 / 125) * h.value * proportion -
                (h.value * proportion - h.value * wLimitProportion) / 2 +
                "px";
        } else {
            limitWidth.value = "100vw";
        }
    }
};

// 当点击各个图片对应事件
const iconClick = (id) => {
    if (id > 1) {
        window.open(iconLink.value[id - 1].url);
    } else if (id < 1) {
        window.open(iconLink.value[id].url);
    } else {
        console.log("open");
        emit("vxOpen", true);
    }
};
</script>

<template>
    <div class="p7Footer" :style="{ width: wp7Footer, left: lp7Footer }">
        <!-- 四个图标 -->
        <img
            ref="footerIcon"
            @click="iconClick(item.id)"
            :id="'fotterIconBg' + item.id"
            class="fotterIconBg"
            v-for="item in footerIcons"
            :key="item.id"
            :src="item.url"
            alt=""
            draggable="false"
        />
    </div>
</template>

<style scoped>
.p7Footer {
    position: absolute;
    left: 0;
    height: 100%;
    /* background-color: rgba(255, 0, 0, 0.1); */
}
.fotterIconBg {
    height: 100%;
}
.fotterIconBg:nth-child(1) {
    margin-right: 45px;
    height: 100%;
}
.fotterIconBg:nth-child(2) {
    margin-right: 35px;
    height: 100%;
}
.fotterIconBg:nth-child(3) {
    margin-right: 35px;
    height: 100%;
}
.fotterIconBg:nth-child(4) {
    margin-right: 0px;
    height: 100%;
}
</style>
