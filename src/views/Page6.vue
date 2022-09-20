<script setup>
import { onUnmounted, onMounted, ref, watch } from "vue";

//窗口的宽度和高度
let w2 = ref(0);
let h2 = ref(0);
const limitWidth = ref("");

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

//各部件的自适应变量
let rNewEvents = ref("");
let wNewEvents = ref("");
let tNewEvents = ref("");
let hNewEventsDetal = ref("");
let lNewEventsDetal = ref("");
let wNewEventsDetal = ref("");
let mteventDetalBg = ref("");

//"更多活动"数据
let events = ref([
    {
        id: 0,
        activeState: true,
        flex: 4.5,
        eventURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/b59b5d6885a0c92d516f89fc38f90ebe_3351754255212541771.png",
        eventActiveURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/b59b5d6885a0c92d516f89fc38f90ebe_3351754255212541771.png",
        eventDetalURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/1cc8d6b5f9f659ff4d2c3891860c2dd6_6890387721119263026.png",
    },
    {
        id: 1,
        activeState: false,
        flex: 1,
        eventURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/9a2fe512e3b8ae7e5877c26a4b0d151e_7974069233215803670.png",
        eventActiveURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/fe3e7b4c16ba321b487ffd6ce920f273_4417421236113259158.png",
        eventDetalURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/ee74130295a5d826c72cd01cd1f866bf_1560797191354503249.png",
    },
    {
        id: 2,
        activeState: false,
        flex: 1,
        eventURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/4b5efa61d76cd2ce82355417b3ed2e56_3284209132296395482.png",
        eventActiveURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/31c5827313d7924329efe69405814bf2_7439800127827989207.png",
        eventDetalURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/b9a283a397bcf6e201481e6468cfa564_4271899804712061220.png",
    },
    {
        id: 3,
        activeState: false,
        flex: 1,
        eventURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/1c1b381850b57623721668d05b3b55da_6581223046693611588.png",
        eventActiveURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/2bb821ee2e5de7cc3c09945524e9e7f6_1340704326728533382.png",
        eventDetalURL:
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/1f67eaa8b16f7c8e1bd8c8a58b4af64b_2616885825390163302.png",
    },
]);
let eventChangeTemp = ref("");

onMounted(() => {
    page6ZSY();
    window.addEventListener("resize", page6ZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", page6ZSY);
});

//自适应函数
const page6ZSY = () => {
    w2.value = document.documentElement.clientWidth;
    h2.value = document.documentElement.clientHeight;
    if (w2.value / h2.value >= proportion) {
        limitWidth.value = "100vw";
        //右上全新大世界
        rNewEvents.value = 0.145 * w2.value + "px";
        wNewEvents.value = 0.065 * w2.value + "px";
        tNewEvents.value = 0 - (w2.value / proportion - h2.value) / 2 + "px";
        //中间板块
        hNewEventsDetal.value = (0.7 * w2.value) / proportion + "px";
        wNewEventsDetal.value = 0.52 * w2.value + "px";
        lNewEventsDetal.value = 0.245 * w2.value + "px";
        //
        mteventDetalBg.value = ((58 / 540) * w2.value) / proportion + "px";
    } else {
        //右上全新大世界
        rNewEvents.value =
            0.145 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        wNewEvents.value = 0.065 * h2.value * proportion + "px";
        tNewEvents.value = 0 + "px";
        //中间板块
        hNewEventsDetal.value = 0.7 * h2.value + "px";
        wNewEventsDetal.value = 0.52 * h2.value * proportion + "px";
        lNewEventsDetal.value =
            0.245 * h2.value * proportion - (h2.value * proportion - w2.value) / 2 + "px";
        //
        mteventDetalBg.value = (58 / 540) * h2.value + "px";
        //限制区域
        if (w2.value / h2.value <= wLimitProportion) {
            limitWidth.value = h2.value * wLimitProportion + "px";
            rNewEvents.value =
                0.145 * h2.value * proportion -
                (h2.value * proportion + h2.value * wLimitProportion - 2 * w2.value) / 2 +
                "px";
            lNewEventsDetal.value =
                0.245 * h2.value * proportion -
                (h2.value * proportion - h2.value * wLimitProportion) / 2 +
                "px";
        } else {
            limitWidth.value = "100vw";
        }
    }
};

//右侧卡片时,改变激活状态和卡片图片,以及鼠标经过时的改变
const eventClick = (id) => {
    if (events.value[id].activeState != true) {
        for (let i = 0; i < events.value.length; i++) {
            events.value[i].flex = 1;
            events.value[i].activeState = false;
        }
        evenChange(id);
        events.value[id].flex = 4.5;
        events.value[id].activeState = true;
    }
};

const eventOver = (id) => {
    if (events.value[id].activeState != true) {
        eventChangeTemp.value = events.value[id].eventURL;
        events.value[id].eventURL = events.value[id].eventActiveURL;
    }
};

const eventLeave = (id) => {
    if (events.value[id].activeState != true) {
        events.value[id].eventURL = eventChangeTemp.value;
    }
};

const evenChange = (id) => {
    for (let i = 0; i < events.value.length; i++) {
        switch (i) {
            case 0:
                events.value[0].eventURL =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/0b12d32aca3f4158743cdcac49244b35_8868651256798534505.png";

                break;
            case 1:
                events.value[1].eventURL =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/9a2fe512e3b8ae7e5877c26a4b0d151e_7974069233215803670.png";

                break;
            case 2:
                events.value[2].eventURL =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/4b5efa61d76cd2ce82355417b3ed2e56_3284209132296395482.png";

                break;
            case 3:
                events.value[3].eventURL =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/1c1b381850b57623721668d05b3b55da_6581223046693611588.png";

                break;

            default:
                break;
        }
    }
    switch (id) {
        case 0:
            events.value[0].eventURL = events.value[0].eventActiveURL;

            break;
        case 1:
            events.value[1].eventURL = events.value[1].eventActiveURL;

            break;
        case 2:
            events.value[2].eventURL = events.value[2].eventActiveURL;

            break;
        case 3:
            events.value[3].eventURL = events.value[3].eventActiveURL;

            break;

        default:
            break;
    }
};
</script>

<template>
    <div class="page6">
        <div class="wlimit" :style="{ width: limitWidth }">
            <!-- 右上角“更多活动”书签 -->
            <img
                class="newEvents"
                :style="{ top: tNewEvents, right: rNewEvents, width: wNewEvents }"
                src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/ee3dbffcabe1d5cc1f5e6cd4cf79cc47_8759166848459933851.png"
                draggable="false"
                alt=""
            />
            <!-- 中间部分 -->
            <div
                class="newEventsDetal"
                :style="{ width: wNewEventsDetal, left: lNewEventsDetal, height: hNewEventsDetal }"
            >
                <!-- 每一个卡片 -->
                <div
                    class="eventSlide"
                    v-for="item in events"
                    :key="item.id"
                    :style="{ flex: item.flex }"
                    @click="eventClick(item.id)"
                >
                    <img
                        class="eventBg"
                        :src="item.eventURL"
                        alt=""
                        draggable="false"
                        @mouseover="eventOver(item.id)"
                        @mouseleave="eventLeave(item.id)"
                    />
                    <div class="eventDetal">
                        <img
                            class="eventDetalBg"
                            :src="item.eventDetalURL"
                            alt=""
                            draggable="false"
                            :style="{ marginTop: mteventDetalBg }"
                        />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.page6 {
    position: relative;
    width: 100%;
    height: 100%;
}

.page6 .wlimit {
    height: 100%;
    background: url("https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/19/638252577be60f43de8bd48801185965_9018799543462188442.png")
        center center no-repeat;
    background-size: cover;
}

.page6 .wlimit .newEvents {
    position: absolute;
    right: 0;
    top: 0;
    width: 72px;
}

.page6 .wlimit .newEventsDetal {
    position: absolute;
    top: 50%;
    width: 520px;
    height: 375px;
    transform: translate(0, -50%);
    /* overflow: hidden; */
    display: flex;
    justify-content: left;
    align-content: center;
}

.eventSlide {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: left;
    align-content: center;
    overflow: hidden;
    transition: 0.3s ease;
}

.eventBg {
    height: 100%;
}

.eventDetal {
    height: 100%;
    width: 100%;
}

.eventDetalBg {
    height: 70%;
    margin-top: 58px;
}
</style>
