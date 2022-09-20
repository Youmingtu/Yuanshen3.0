<script setup>
import { onUnmounted, onMounted, ref } from "vue";

import Swiper, { Pagination, EffectCoverflow, Autoplay, Navigation, Mousewheel } from "swiper";
import "../../node_modules/swiper/swiper-bundle.min.css";
import "../../node_modules/swiper/swiper-bundle.min.js";
import "../../node_modules/swiper/swiper-bundle.js";

//各部件的自适应变量
let w21 = ref(0);
let h21 = ref(0);
let hArrow = ref("");
let lLeftArrow = ref("");
let lRightArrow = ref("");
let wSlide = ref("");
let distance = ref(0);
let hideDistance = ref(0);
let tPaginations = ref("");
let lPaginations = ref("");
let hPaginationBullets = ref("");
let mrPaginationBullets = ref("");

//记录上一个激活的slide,是否可以切换slide
let canChange = ref(true);
let prevId = ref(0);

//左右箭头背景图,中间slide图片和底下分页器的图片数据
const leftArrowChangeBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/50364fa251f18c5f80b06ccd51ae9acc_4784054183997258888.png",
);
const rightArrowChangeBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/14a1e37929cd46201fc705fa5c976bfc_2627128240072259295.png",
);
let charactreristics = ref([
    {
        id: 0,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/ccacb5a63ec19ef530a8d37466ec8081_3788366410575723900.png",
    },
    {
        id: 1,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/ef869fe8922420e51a8c8c7e243e5fcb_6537265619450863950.png",
    },
    {
        id: 2,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/7f65803288bd65cef94c5b06e0f78e8a_4460669545671865655.png",
    },
    {
        id: 3,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/26d4160f5850e171516e58d5988734c2_3134515439080342909.png",
    },
    {
        id: 4,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/799b62d43579b9e7052c9358b8e1a274_4152449037409605667.png",
    },
]);
let p3Paginations = ref([
    {
        id: 0,
        active: true,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/ee9d3a102327262f008e360a4b34e9e7_2210792581546549000.png",
    },
    {
        id: 1,
        active: false,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/92857e31a7fc66f2f143969f85655945_3677178573951829721.png",
    },
    {
        id: 2,
        active: false,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/92857e31a7fc66f2f143969f85655945_3677178573951829721.png",
    },
    {
        id: 3,
        active: false,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/92857e31a7fc66f2f143969f85655945_3677178573951829721.png",
    },
    {
        id: 4,
        active: false,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/92857e31a7fc66f2f143969f85655945_3677178573951829721.png",
    },
]);

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

// swiper设置
let page7SwiperEvent = ref();
let page7SwiperOP = ref({
    direction: "horizontal", //滑动方向
    speed: 300, //滑动速度
    loop: true,
    loopAdditionalSlides: 4,
    slidesPerView: "auto", //滑动到贴合边缘
    centeredSlides: true,
    mousewheel: false, //使用鼠标滚轮
    allowTouchMove: false, //不允许触屏拖动
    observer: true, // 监听
    observeParents: true, // 监听
    nested: true, //子组件标志
    resistanceRatio: 0,
    watchSlidesProgress: true,

    autoplay: {
        delay: 7000,
        disableOnInteraction: false, //触碰后不停止自动切换（有时候可能点击一下就卡住了）
        pauseOnMouseEnter: false, //鼠标放上去时是否停止自动切换
    },

    navigation: {
        nextEl: ".swiper-button-next",
        prevEl: ".swiper-button-prev",
    },
});

onMounted(() => {
    Swiper.use([Pagination, EffectCoverflow, Autoplay, Navigation, Mousewheel]);
    const page7Swiper = new Swiper(".page7Swiper", page7SwiperOP.value);

    page7SwiperEvent.value = page7Swiper; //复制以使用swiper方法

    //swiper切换时进度(progress)
    page7Swiper.on("progress", (swiper, progress) => {
        swiperProgress(swiper, progress);
    });

    //swiper切换的动作
    page7Swiper.on("setTransition", (swiper, transition) => {
        for (var i = 0; i < swiper.slides.length; i++) {
            swiper.slides[i].style.transition =
                "all" + " " + transition / 1000 + "s" + " " + "ease";
        }
    });

    //slide切换时
    page7Swiper.on("slideChange", (swiper) => {
        setTimeout(() => {
            maskOpacityChange(swiper);
        }, 10);
    });

    // swiper开始切换时
    page7Swiper.on("transitionStart", (swiper) => {
        canChange.value = false;
        setTimeout(() => {
            prevId.value = page7SwiperEvent.value.activeIndex % 5;
            paginationActiveChange(page7SwiperEvent.value.realIndex);
        }, 10);
    });

    // swiper结束切换时
    page7Swiper.on("transitionEnd", (swiper) => {
        canChange.value = true;
    });

    //每一个slide都有一层较暗的蒙版层,slide切换时改变对应的蒙版层
    maskOpacityChange(page7Swiper);

    slideZSY();
    window.addEventListener("resize", slideZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", slideZSY);
});

// 自适应函数
const slideZSY = () => {
    w21.value = document.documentElement.clientWidth;
    h21.value = document.documentElement.clientHeight;
    if (w21.value / h21.value >= proportion) {
        //左右箭头
        hArrow.value = (0.15 * w21.value) / proportion + "px";
        lLeftArrow.value = 0.23 * w21.value + "px";
        lRightArrow.value = 0.705 * w21.value + "px";
        //slide
        for (let i = 0; i < page7SwiperEvent.value.slides.length; i++) {
            page7SwiperEvent.value.slides[i].style.width = (52 / 125) * w21.value + "px";
        }
        //progree distance
        distance.value = (52 / 125) * w21.value * 0.75;
        hideDistance.value = (50 / 125) * w21.value;
        //分页器
        tPaginations.value = ((290 / 540) * w21.value) / proportion + "px";
        lPaginations.value = (55 / 125) * w21.value + "px";
        //分页器大小
        hPaginationBullets.value = ((7 / 270) * w21.value) / proportion + "px";
        mrPaginationBullets.value =
            "0" + " " + ((25 / 540) * w21.value) / proportion + "px" + " " + "0" + " " + "0";
    } else {
        //左右箭头
        hArrow.value = 0.15 * h21.value + "px";
        lLeftArrow.value =
            0.23 * h21.value * proportion - (h21.value * proportion - w21.value) / 2 + "px";
        lRightArrow.value =
            0.705 * h21.value * proportion - (h21.value * proportion - w21.value) / 2 + "px";
        //slide
        for (let i = 0; i < page7SwiperEvent.value.slides.length; i++) {
            page7SwiperEvent.value.slides[i].style.width =
                (52 / 125) * h21.value * proportion + "px";
        }
        //progree distance
        distance.value = (52 / 125) * h21.value * proportion * 0.75;
        hideDistance.value = (50 / 125) * h21.value * proportion;
        //分页器
        tPaginations.value = (290 / 540) * h21.value + "px";
        lPaginations.value =
            (55 / 125) * h21.value * proportion - (h21.value * proportion - w21.value) / 2 + "px";
        //分页器大小
        hPaginationBullets.value = (7 / 270) * h21.value + "px";
        mrPaginationBullets.value =
            "0" + " " + (25 / 540) * h21.value + "px" + " " + "0" + " " + "0";
        //限制区域
        if (w21.value / h21.value <= wLimitProportion) {
        } else {
        }
    }
};

//swiper切换效果(卡片轮播),放到swiper切换进度调用
const swiperProgress = (swiper, progress) => {
    for (let i = 0; i < swiper.slides.length; i++) {
        var slide = swiper.slides.eq(i);
        var slideProgress = swiper.slides[i].progress;
        let modify = 1; //移动倍数
        if (Math.abs(slideProgress) > 1) {
            modify = (Math.abs(slideProgress) - 1) * 0.3 + 1;
        }
        if (modify >= 1.6) modify = 1.6;
        let translate = "";
        if (Math.abs(slideProgress) > 1) {
            translate =
                (Math.abs(slideProgress) / slideProgress) *
                    ((Math.abs(slideProgress) - 1) * hideDistance.value + 1 * distance.value) +
                "px";
        } else {
            translate = slideProgress * modify * distance.value + "px";
        }
        let scale = 1 - Math.abs(slideProgress) / 5;
        let zIndex = 99 - Math.abs(Math.round(10 * slideProgress));
        slide.transform("translateX(" + translate + ") scale(" + scale + ")");
        slide.css("zIndex", zIndex);
        slide.css("opacity", 1);
        if (Math.abs(slideProgress) > 1.5) {
            slide.css("opacity", 0);
        }
    }
};

//改变蒙版层
const maskOpacityChange = (swiper) => {
    for (let i = 0; i < swiper.slides.length; i++) {
        swiper.slides[i].firstChild.style.opacity = 1;
    }
    if (swiper.activeIndex < 10) {
        swiper.slides[swiper.activeIndex].firstChild.style.opacity = 0;
        // console.log(swiper.activeIndex);
        swiper.slides[swiper.activeIndex + 5].firstChild.style.opacity = 0;
    }
    if (swiper.activeIndex >= 10) {
        swiper.slides[swiper.activeIndex].firstChild.style.opacity = 0;
        swiper.slides[swiper.activeIndex - 5].firstChild.style.opacity = 0;
    }
};

// 左右按钮
const prevSlide = () => {
    if (canChange.value) {
        console.log("prevent");
        page7SwiperEvent.value.slidePrev();
    }
};

const nextSlide = () => {
    if (canChange.value) {
        console.log("next");
        page7SwiperEvent.value.slideNext();
    }
};

const leftArrowOver = () => {
    leftArrowChangeBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/5a9a25ff9ebb0e91c92fb5e7a909a6ee_4138268398773976711.png";
};

const leftArrowLeave = () => {
    leftArrowChangeBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/50364fa251f18c5f80b06ccd51ae9acc_4784054183997258888.png";
};

const rightArrowOver = () => {
    rightArrowChangeBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/7110b8cb2a5171f20980cbda08c42e60_7320709047166520913.png";
};

const rightArrowLeave = () => {
    rightArrowChangeBg.value =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/14a1e37929cd46201fc705fa5c976bfc_2627128240072259295.png";
};

//分页器
const paginationClick = (id) => {
    console.log(
        "active:",
        page7SwiperEvent.value.activeIndex,
        "prevId:",
        prevId.value,
        "clickId:",
        id,
    );
    if (p3Paginations.value[id].active == false && canChange.value == true) {
        // page7SwiperEvent.value.slideTo(id);
        // if()
        if (id - prevId.value == 1 || id - prevId.value == -4) {
            page7SwiperEvent.value.slideNext();
        } else if (id - prevId.value == -1 || id - prevId.value == 4) {
            page7SwiperEvent.value.slidePrev();
            // } else if (prevId.value == 0 && id == 4) {
            //     page7SwiperEvent.value.slidePrev();
            // } else if (prevId.value == 4 && id == 0) {
            //     page7SwiperEvent.value.slideNext();
        } else if (prevId.value == 0 && id <= 3) {
            page7SwiperEvent.value.slideNext();
            page7SwiperEvent.value.slideTo(page7SwiperEvent.value.activeIndex + id - 1);
        } else if (prevId.value == 0 && id > 3) {
            page7SwiperEvent.value.slidePrev();
            page7SwiperEvent.value.slideTo(page7SwiperEvent.value.activeIndex - (4 - id));
        } else if (prevId.value == 4 && id < 3) {
            page7SwiperEvent.value.slideNext();
            page7SwiperEvent.value.slideTo(page7SwiperEvent.value.activeIndex + id);
        } else if (prevId.value == 4 && id >= 3) {
            page7SwiperEvent.value.slidePrev();
            page7SwiperEvent.value.slideTo(
                page7SwiperEvent.value.activeIndex - (prevId.value - id - 1),
            );
        } else if (id - prevId.value > 0) {
            page7SwiperEvent.value.slideNext();
            page7SwiperEvent.value.slideTo(
                page7SwiperEvent.value.activeIndex + (id - prevId.value - 1),
            );
        } else if (id - prevId.value < 0)
            page7SwiperEvent.value.slidePrev(),
                page7SwiperEvent.value.slideTo(
                    page7SwiperEvent.value.activeIndex + (id - prevId.value + 1),
                );
        prevId.value = id;
    }
};

const paginationActiveChange = (index) => {
    for (let i = 0; i < p3Paginations.value.length; i++) {
        p3Paginations.value[i].active = false;
        p3Paginations.value[i].url =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/92857e31a7fc66f2f143969f85655945_3677178573951829721.png";
    }
    p3Paginations.value[index].active = true;
    p3Paginations.value[index].url =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/ee9d3a102327262f008e360a4b34e9e7_2210792581546549000.png";
    // }
};
</script>

<template>
    <div class="swiper page7Swiper">
        <div class="swiper-wrapper">
            <div class="swiper-slide p7Slide" v-for="item in charactreristics" :key="item.id">
                <div class="mask"></div>
                <img class="slideWidth" :src="item.url" alt="" draggable="false" />
            </div>
        </div>
    </div>
    <!-- 前进后退按钮 -->
    <div class="leftArrow" @click="prevSlide" :style="{ left: lLeftArrow, height: hArrow }">
        <img
            @mouseover="leftArrowOver"
            @mouseleave="leftArrowLeave"
            class="leftArrowBg"
            :src="leftArrowChangeBg"
            draggable="false"
            alt=""
        />
    </div>
    <div class="rightArrow" @click="nextSlide" :style="{ left: lRightArrow, height: hArrow }">
        <img
            @mouseover="rightArrowOver"
            @mouseleave="rightArrowLeave"
            class="rightArrowBg"
            :src="rightArrowChangeBg"
            draggable="false"
            alt=""
        />
    </div>
    <!-- 分页器 -->
    <div class="paginations" :style="{ top: tPaginations, left: lPaginations }">
        <img
            class="paginationBullets"
            :style="{
                width: hPaginationBullets,
                height: hPaginationBullets,
                margin: mrPaginationBullets,
            }"
            v-for="item in p3Paginations"
            :key="item.id"
            :src="item.url"
            @click="paginationClick(item.id)"
            alt=""
            draggable="false"
        />
    </div>
</template>

<style scoped>
.page7Swiper {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    /* background-color: rgba(255, 0, 0, 0.2); */
    overflow: visible;
}

.p7Slide {
    height: 100%;
    /* background-color: rgba(0, 255, 0, 0.5); */
}

.slideWidth {
    width: 100%;
}

.mask {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    transition: 0.3s ease;
    opacity: 1;
}

.leftArrow {
    position: absolute;
    left: 130px;
    top: 37%;
    width: 50px;
    height: 30px;
    z-index: 1;
}

.leftArrow .leftArrowBg {
    height: 100%;
}

.rightArrow {
    position: absolute;
    left: 295px;
    top: 37%;
    width: 50px;
    height: 30px;
    z-index: 1;
}

.rightArrow .rightArrowBg {
    height: 100%;
}

.page7Swiper .swiper-wrapper .slideHeight {
    height: 100%;
}

.paginations {
    position: absolute;
    top: 484px;
    left: 192px;
    display: flex;
    align-content: center;
    /* background-color: rgba(0, 0, 0, 0.1); */
    z-index: 1;
}

.paginationBullets {
    width: 12px;
    height: 12px;
    /* margin-right: 41px; */
    /* background-color: rgba(255, 0, 0, 0.5); */
    z-index: 1;
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
