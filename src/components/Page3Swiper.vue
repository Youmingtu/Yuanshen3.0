<script setup>
import { onUnmounted, onMounted, ref, nextTick, onUpdated, watch, h } from "vue";

import Swiper, { Thumbs, Autoplay, Navigation, EffectFade, Pagination } from "swiper";
import "../../node_modules/swiper/swiper-bundle.min.css";
import "../../node_modules/swiper/swiper-bundle.min.js";

const emit = defineEmits(["videoId"]);

//各部件的自适应变量
let w21 = ref(0);
let h21 = ref(0);
let wSwiper3 = ref("");
let tSwiper3 = ref("");
let hSwiper3Thumbs = ref("");
let wSwiper3Thumbs = ref("");
let lSwiper3Thumbs = ref("");
let tSwiper3Thumbs = ref("");
let hArrow = ref("");
let tArrow = ref("");
let lLeftArrow = ref("");
let lRightArrow = ref("");
let tPaginations = ref("");
let lPaginations = ref("");
let hPaginationBullets = ref("");
let mrPaginationBullets = ref("");
let wThumbsSlide = ref("");
let hThumbsSlide = ref("");
let wCVAudio = ref("");
let hCVAudio = ref("");
let tCVAudio = ref("");
let lCVAudio = ref("");
let fsCVtext = ref("");
let wcharacterVideo = ref("");
let lcharacterVideo = ref("");
let tcharacterVideo = ref("");

let prevId = ref(0); // 记录上一个slide对应的id
let canClikePagination = ref(true); //是否可以点击分页器

// 左右箭头的背景图片
let leftArrowChangeBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/1be0b18a1f2a5843acf8dadb9fe3ada2_6827807411844732625.png",
);
let rightArrowChangeBg = ref(
    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/cc38285677194c5889cefe8954dae9ff_6794306022965478022.png",
);

//标准比例
const proportion = 250 / 108;
const wLimitProportion = 943 / 635;

// 角色数据
let p3Characters = ref([
    {
        id: 0,
        name: "提纳里",
        cnCV: "中文CV：莫然",
        jpCV: "日文CV：小林沙苗",
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/12/7e8c768ced021e643b2745f4fcecb27d_3531858439244942836.png",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2022/08/23/2a98cd5057e4330b3130591df0ce60b5_2023614773460852835.mp4",
    },
    {
        id: 1,
        name: "钟离",
        cnCV: "中文CV：彭博",
        jpCV: "日文CV：前野智昭",
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/724fb897cc35fec3bc32b29f0d7326d4_2753308132007511840.png",
        videoURL:
            "https://uploadstatic.mihoyo.com/hk4e/upload/officialsites/202011/%E9%92%9F%E7%A6%BB%E8%A7%92%E8%89%B2%E6%BC%94%E7%A4%BA_1606702228_4352.mp4",
    },
    {
        id: 2,
        name: "柯莱",
        cnCV: "中文CV：秦文静",
        jpCV: "日文CV：前川凉子",
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/db092adcf3e4464d48f5243a5c0a87e2_4418488720297059705.png",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2022/08/22/1ae2e5f564f6c2c57cb7c494ce9f2440_1076195285047353656.mp4",
    },
    {
        id: 3,
        name: "甘雨",
        cnCV: "中文CV：林簌",
        jpCV: "日文CV：上田丽奈",
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/bc4f7cc233a7aea3d950c89a44c118de_3276787651480242694.png",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2021/01/11/0ede4299ba691f5c011cd1f4a1d0b4fa_4557158493605335930.mp4",
    },
    {
        id: 4,
        name: "心海",
        cnCV: "中文CV：龟娘",
        jpCV: "日文CV：三森铃子",
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/a4957eb7a9d55911316a2814fd68db02_6700919328923513619.png",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2021/09/19/6ad98a93af5a4caf2c8eddfd8bda51d7_3890877391063843086.mp4",
    },
    {
        id: 5,
        name: "多莉",
        cnCV: "中文CV：王晓彤",
        jpCV: "日文CV：金田朋子",
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/5a96e864ef7b7f5a78090b7c03bc0ad8_6760500629550482945.png",
        videoURL:
            "https://webstatic.mihoyo.com/upload/static-resource/2022/09/02/47bd22e2f4b5bde2564a16abc8ba98cc_3449146653470709192.mp4",
    },
]);
let p3CharactersThumbs = ref([
    {
        id: 0,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/3744c9db6b3a226a7bfe18b0f51ecfdf_824486152926644486.png",
    },
    {
        id: 1,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/c5b7a0b139101dcc153a220c84f53a4d_7098013317157684706.png",
    },
    {
        id: 2,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/dd4ea10db9c861da163c2de41b3ec5e2_1473513864377561881.png",
    },
    {
        id: 3,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/8856c753e04110b170ab9bcc1aa2200b_1250985052010941710.png",
    },
    {
        id: 4,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/3f65bd5d784c8915881371a7b90668a1_5420157117093017099.png",
    },
    {
        id: 5,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/ee704bb4199151ff7d266fef2383cf43_6299920457464254775.png",
    },
]);
let p3Paginations = ref([
    {
        id: 0,
        active: true,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/efadc9a8139b73742b3354bc399f1f6d_7756066622040347862.png",
    },
    {
        id: 1,
        active: false,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/febe05f8cbeada8a8caf2683abeb347f_7645033237024312925.png",
    },
    {
        id: 2,
        active: false,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/febe05f8cbeada8a8caf2683abeb347f_7645033237024312925.png",
    },
    {
        id: 3,
        active: false,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/febe05f8cbeada8a8caf2683abeb347f_7645033237024312925.png",
    },
    {
        id: 4,
        active: false,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/febe05f8cbeada8a8caf2683abeb347f_7645033237024312925.png",
    },
    {
        id: 5,
        active: false,
        url: "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/febe05f8cbeada8a8caf2683abeb347f_7645033237024312925.png",
    },
]);

// 接收swiper方法
const swiper3Event = ref();
const swiper3ThumbEvent = ref();

// swiper设置
let swiper3Op = ref({
    direction: "horizontal", // 垂直切换选项
    speed: 400,
    // loop: true, // 循环模式选项
    // loopAdditionalSlides: 4,
    // slidesPerView: 3,
    allowTouchMove: false, //无法触屏滑动
    observer: true,
    // observeSlideChildren: true,
    nested: true, //子组件标志
    resistanceRatio: 0, //反弹力大小
    // updateOnWindowResize: true, //禁止slide随着窗口宽度变化（因为可能会出问题）
    preventInteractionOnTransition: true, //过渡时无法切换
    loopPreventsSlide: true, //过渡时无法切换

    // 如果需要前进后退按钮
    navigation: {
        nextEl: ".swiper-button-next",
        prevEl: ".swiper-button-prev",
    },

    // autoplay: {
    //     delay: 500,
    // },

    effect: "fade",
    fadeEffect: {
        crossFade: true,
    },
});
let swiper3ThumosOp = ref({
    direction: "horizontal", // 垂直切换选项
    speed: 400,
    loop: true, // 循环模式选项
    loopAdditionalSlides: 4,
    slidesPerView: 3,
    centeredSlides: true, //居中显示
    // slideToClickedSlide: true, //点击到slide跳转
    slideActiveClass: "pcActive",
    allowTouchMove: false, //无法触屏滑动
    // preventInteractionOnTransition: true, //过渡时无法切换
    // loopPreventsSlide: true, //过渡时无法切换

    autoplay: {
        delay: 6000,
    },
});
Swiper.use([Thumbs, Autoplay, Navigation, EffectFade, Pagination]);
onMounted(() => {
    var mySwiper1 = new Swiper(".swiper3", swiper3Op.value);
    var mySwiper2 = new Swiper(".swiper3Thumbs", swiper3ThumosOp.value);

    swiper3Event.value = mySwiper1;
    swiper3ThumbEvent.value = mySwiper2;

    // swiper开始切换时
    swiper3Event.value.on("transitionStart", function (swiper) {
        setTimeout(() => {
            paginationActiveChange(swiper3Event.value.realIndex);
        }, 10);
    });
    swiper3ThumbEvent.value.on("transitionStart", function (swiper) {
        canClikePagination.value = false;
        setTimeout(() => {
            setTimeout(() => {
                canClikePagination.value = true;
            }, 400);
            // paginationActiveChange(swiper3Event.value.realIndex);
            thumbsActiveChange(swiper.realIndex);
            prevId.value = swiper3ThumbEvent.value.activeIndex % 6;
            swiper3Event.value.slideTo(swiper.realIndex);
        }, 10);
    });

    //点击slide时跳到那一页
    swiper3ThumbEvent.value.on("tap", function (swiper) {
        setTimeout(() => {
            // paginationActiveChange(swiper3Event.value.realIndex);
            if (swiper.clickedIndex > swiper.activeIndex) {
                swiper.slideNext();
                if (swiper3Event.value.realIndex == 5) swiper3Event.value.slideTo(0, 400);
                else swiper3Event.value.slideNext();
            }
            if (swiper.clickedIndex < swiper.activeIndex) {
                swiper.slidePrev();
                if (swiper3Event.value.realIndex == 0) swiper3Event.value.slideTo(5, 400);
                else swiper3Event.value.slidePrev();
            }
        }, 1);
    });

    slideZSY();
    window.addEventListener("resize", slideZSY);
});

onUnmounted(() => {
    window.removeEventListener("resize", slideZSY);
});

//自适应函数
const slideZSY = () => {
    w21.value = document.documentElement.clientWidth;
    h21.value = document.documentElement.clientHeight;
    if (w21.value / h21.value >= proportion) {
        //主要swiper
        wSwiper3.value = 0.543 * w21.value + "px";
        tSwiper3.value =
            (0.033 * w21.value) / proportion - (w21.value / proportion - h21.value) / 2 + "px";
        //缩略图
        hSwiper3Thumbs.value = (0.0965 * w21.value) / proportion + "px";
        wSwiper3Thumbs.value = (3 * 0.0965 * w21.value) / proportion + "px";
        lSwiper3Thumbs.value = (16 / 300) * w21.value + "px";
        tSwiper3Thumbs.value =
            (0.68 * w21.value) / proportion - (w21.value / proportion - h21.value) / 2 + "px";
        //缩略图slide
        // wThumbsSlide.value = 0.0965 * h21.value + "px";
        for (let i = 0; i < swiper3ThumbEvent.value.slides.length; i++) {
            swiper3ThumbEvent.value.slides[i].style.width =
                (0.0965 * w21.value) / proportion + "px";
        }
        //左右箭头
        hArrow.value = 0.025 * w21.value + "px";
        tArrow.value =
            (0.7 * w21.value) / proportion - (w21.value / proportion - h21.value) / 2 + "px";
        lLeftArrow.value = 0.032 * w21.value + "px";
        lRightArrow.value = 0.175 * w21.value + "px";
        //分页器
        tPaginations.value =
            ((484 / 540) * w21.value) / proportion -
            (w21.value / proportion - h21.value) / 2 +
            "px";
        lPaginations.value = (192 / 1250) * w21.value + "px";
        //分页器大小
        hPaginationBullets.value = ((6 / 270) * w21.value) / proportion + "px";
        mrPaginationBullets.value =
            "0" + " " + ((41 / 540) * w21.value) / proportion + "px" + " " + "0" + " " + "0";
        //CV
        hCVAudio.value = ((8 / 54) * w21.value) / proportion + "px";
        wCVAudio.value = ((43 / 125) * w21.value) / proportion + "px";
        lCVAudio.value = (44 / 1250) * w21.value + "px";
        tCVAudio.value = ((24 / 54) * w21.value) / proportion + "px";
        //cv字体大小
        fsCVtext.value = (12 / 1250) * w21.value + "px";
        //角色视频按钮
        wcharacterVideo.value = ((10 / 125) * w21.value) / proportion + "px";
        lcharacterVideo.value = (23 / 125) * w21.value + "px";
        tcharacterVideo.value = ((125 / 540) * w21.value) / proportion + "px";
    } else {
        //主要swiper
        wSwiper3.value = 0.543 * h21.value * proportion + "px";
        tSwiper3.value = 0.033 * h21.value + "px";
        //缩略图
        hSwiper3Thumbs.value = 0.0965 * h21.value + "px";
        wSwiper3Thumbs.value = 3 * 0.0965 * h21.value + "px";
        lSwiper3Thumbs.value = (16 / 300) * h21.value * proportion + "px";
        tSwiper3Thumbs.value = 0.68 * h21.value + "px";
        //缩略图slide
        for (let i = 0; i < swiper3ThumbEvent.value.slides.length; i++) {
            // swiper3Event.value.thumbs.swiper.slides[i].style.width = 65 + "px";
            swiper3ThumbEvent.value.slides[i].style.width = 0.0965 * h21.value + "px";
        }
        // wThumbsSlide.value = 0.0965 * h21.value + "px";
        //左右箭头
        hArrow.value = 0.025 * h21.value * proportion + "px";
        tArrow.value = 0.7 * h21.value + "px";
        lLeftArrow.value = 0.032 * h21.value * proportion + "px";
        lRightArrow.value = 0.175 * h21.value * proportion + "px";
        //分页器
        tPaginations.value = (484 / 540) * h21.value + "px";
        lPaginations.value = (192 / 1250) * h21.value * proportion + "px";
        //分页器大小
        hPaginationBullets.value = (6 / 270) * h21.value + "px";
        mrPaginationBullets.value =
            "0" + " " + (41 / 540) * h21.value + "px" + " " + "0" + " " + "0";
        //CV
        hCVAudio.value = (8 / 54) * h21.value + "px";
        wCVAudio.value = (43 / 125) * h21.value + "px";
        lCVAudio.value = (44 / 1250) * h21.value * proportion + "px";
        tCVAudio.value = (24 / 54) * h21.value + "px";
        //cv字体大小
        fsCVtext.value = (12 / 1250) * h21.value * proportion + "px";
        //角色视频按钮
        wcharacterVideo.value = (10 / 125) * h21.value + "px";
        lcharacterVideo.value = (23 / 125) * h21.value * proportion + "px";
        tcharacterVideo.value = (125 / 540) * h21.value + "px";
        //限制区域
        if (w21.value / h21.value <= wLimitProportion) {
        } else {
        }
    }
};

//左右箭头
const prevSlide = () => {
    console.log("prev");
    if (swiper3Event.value.realIndex == 0) swiper3Event.value.slideTo(5, 400);
    else swiper3Event.value.slidePrev();
    swiper3ThumbEvent.value.slidePrev();
    // swiper3Event.value.thumbs.swiper.slidePrev();
};
const nextSlide = () => {
    console.log("next");
    if (swiper3Event.value.realIndex == 5) swiper3Event.value.slideTo(0, 400);
    else swiper3Event.value.slideNext();
    swiper3ThumbEvent.value.slideNext();
    // swiper3Event.value.thumbs.swiper.slideNext();
};

// 控制左下缩略图(头像)变化
const thumbsActiveChange = (realIndex) => {
    for (let i = 0; i < p3CharactersThumbs.value.length; i++) {
        switch (i) {
            case 0:
                p3CharactersThumbs.value[0].url =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/0a63359363e0b581d1def1243eadd9be_8221997924895872957.png";
                continue;
            case 1:
                p3CharactersThumbs.value[1].url =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/c5b7a0b139101dcc153a220c84f53a4d_7098013317157684706.png";
                continue;
            case 2:
                p3CharactersThumbs.value[2].url =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/dd4ea10db9c861da163c2de41b3ec5e2_1473513864377561881.png";
                continue;
            case 3:
                p3CharactersThumbs.value[3].url =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/8856c753e04110b170ab9bcc1aa2200b_1250985052010941710.png";
                continue;
            case 4:
                p3CharactersThumbs.value[4].url =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/3f65bd5d784c8915881371a7b90668a1_5420157117093017099.png";
                continue;
            case 5:
                p3CharactersThumbs.value[5].url =
                    "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/ee704bb4199151ff7d266fef2383cf43_6299920457464254775.png";
                continue;
        }
    }
    switch (realIndex) {
        case 0:
            p3CharactersThumbs.value[0].url =
                "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/3744c9db6b3a226a7bfe18b0f51ecfdf_824486152926644486.png";
            break;
        case 1:
            p3CharactersThumbs.value[1].url =
                "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/2ec21e24eeec4008d2c2d96282d0e40a_1746796043005212550.png";
            break;
        case 2:
            p3CharactersThumbs.value[2].url =
                "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/56f86cb7e7c9dac6d6c2c49e600cd45c_5043785746223927432.png";
            break;
        case 3:
            p3CharactersThumbs.value[3].url =
                "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/7014a4743a15537ed20c0f5e2d1f496f_4866469044011894301.png";
            break;
        case 4:
            p3CharactersThumbs.value[4].url =
                "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/c744905030ebffc1ad700ac225ac2f9f_7089344622809546227.png";
            break;
        case 5:
            p3CharactersThumbs.value[5].url =
                "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/b77d759b961cbc5e53026afa8dd88c63_6834722892894905462.png";
            break;

        default:
            break;
    }
    if (swiper3ThumbEvent.value.realIndex == 0) {
        swiper3ThumbEvent.value.slides[12].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/3744c9db6b3a226a7bfe18b0f51ecfdf_824486152926644486.png";
    } else {
        swiper3ThumbEvent.value.slides[12].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/0a63359363e0b581d1def1243eadd9be_8221997924895872957.png";
    }
    if (swiper3ThumbEvent.value.realIndex == 1) {
        swiper3ThumbEvent.value.slides[13].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/2ec21e24eeec4008d2c2d96282d0e40a_1746796043005212550.png";
    } else {
        swiper3ThumbEvent.value.slides[13].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/c5b7a0b139101dcc153a220c84f53a4d_7098013317157684706.png";
    }
    if (swiper3ThumbEvent.value.realIndex == 2) {
        swiper3ThumbEvent.value.slides[14].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/56f86cb7e7c9dac6d6c2c49e600cd45c_5043785746223927432.png";
    } else {
        swiper3ThumbEvent.value.slides[14].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/dd4ea10db9c861da163c2de41b3ec5e2_1473513864377561881.png";
    }
    if (swiper3ThumbEvent.value.realIndex == 4) {
        swiper3ThumbEvent.value.slides[4].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/c744905030ebffc1ad700ac225ac2f9f_7089344622809546227.png";
    } else {
        swiper3ThumbEvent.value.slides[4].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/3f65bd5d784c8915881371a7b90668a1_5420157117093017099.png";
    }
    if (swiper3ThumbEvent.value.realIndex == 5) {
        swiper3ThumbEvent.value.slides[5].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/b77d759b961cbc5e53026afa8dd88c63_6834722892894905462.png";
    } else {
        swiper3ThumbEvent.value.slides[5].firstElementChild.firstElementChild.src =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/ee704bb4199151ff7d266fef2383cf43_6299920457464254775.png";
    }
};

//点击底下的分页器时
const paginationClick = (id) => {
    if (p3Paginations.value[id].active == false && canClikePagination.value == true) {
        swiper3Event.value.slideTo(id);
        // if()
        if (id - prevId.value == 1 || id - prevId.value == -5) {
            swiper3ThumbEvent.value.slideNext();
        } else if (id - prevId.value == -1 || id - prevId.value == 5) {
            swiper3ThumbEvent.value.slidePrev();
        } else if (prevId.value == 0 && id == 5) {
            swiper3ThumbEvent.value.slidePrev();
        } else if (prevId.value == 5 && id == 0) {
            swiper3ThumbEvent.value.slideNext();
        } else if (prevId.value == 0 && id <= 3) {
            swiper3ThumbEvent.value.slideNext();
            swiper3ThumbEvent.value.slideTo(swiper3ThumbEvent.value.activeIndex + id - 1);
        } else if (prevId.value == 0 && id > 3) {
            swiper3ThumbEvent.value.slidePrev();
            swiper3ThumbEvent.value.slideTo(swiper3ThumbEvent.value.activeIndex - (5 - id));
        } else if (prevId.value == 5 && id < 3) {
            swiper3ThumbEvent.value.slideNext();
            swiper3ThumbEvent.value.slideTo(swiper3ThumbEvent.value.activeIndex + id);
        } else if (prevId.value == 5 && id >= 3) {
            swiper3ThumbEvent.value.slidePrev();
            swiper3ThumbEvent.value.slideTo(
                swiper3ThumbEvent.value.activeIndex - (prevId.value - id - 1),
            );
        } else if (id - prevId.value > 0) {
            swiper3ThumbEvent.value.slideNext();
            swiper3ThumbEvent.value.slideTo(
                swiper3ThumbEvent.value.activeIndex + (id - prevId.value - 1),
            );
        } else if (id - prevId.value < 0)
            swiper3ThumbEvent.value.slidePrev(),
                swiper3ThumbEvent.value.slideTo(
                    swiper3ThumbEvent.value.activeIndex + (id - prevId.value + 1),
                );
        prevId.value = id;
    }
};

//底下的分页器变化
const paginationActiveChange = (index) => {
    for (let i = 0; i < p3Paginations.value.length; i++) {
        p3Paginations.value[i].active = false;
        p3Paginations.value[i].url =
            "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/febe05f8cbeada8a8caf2683abeb347f_7645033237024312925.png";
    }
    p3Paginations.value[index].active = true;
    p3Paginations.value[index].url =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/16/efadc9a8139b73742b3354bc399f1f6d_7756066622040347862.png";
    // }
};

// 点击音频播放时
const audioClick = (e) => {
    alert("没有音频api    ≧ ﹏ ≦");
};

// 音频按钮的mouseover和mouseleave
const audioOver = (e) => {
    e.target.src =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/1ee5beb613759d82f422504d3513bf7b_1851948923015713524.png";
};

const audioOut = (e) => {
    e.target.src =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/9cf776446c743f0e7efaa5d85d20a39b_6523050722555489025.png";
};

// 点击视频按钮时
const videoClick = (id) => {
    emit("videoId", id);
    console.log("发送id", id);
};

// 视频按钮的mouseover和mouseleave
const videoOver = (e) => {
    e.target.src =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/67e39afa889f9f6d1a25dd08f1ab98de_8662726075779531281.png";
};

const videoOut = (e) => {
    e.target.src =
        "https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/4f95de627d302f84e4256d843f10f444_420562542973104737.png";
};
</script>

<template>
    <!-- 主要swiper -->
    <div class="swiper swiper3" :style="{ width: wSwiper3 }">
        <div class="swiper-wrapper wrapper3" :style="{ top: tSwiper3 }">
            <div class="swiper-slide" v-for="item in p3Characters" :key="item.id">
                <div class="slideWidth">
                    <!-- 角色语音 -->
                    <div
                        class="CVAudio"
                        :style="{
                            width: wCVAudio,
                            height: hCVAudio,
                            top: tCVAudio,
                            left: lCVAudio,
                        }"
                    >
                        <div class="CVAudio1">
                            <p class="CVAudio1-text" :style="{ fontSize: fsCVtext }">
                                {{ item.cnCV }}
                            </p>
                            <img
                                @click="audioClick"
                                @mouseover="audioOver"
                                @mouseout="audioOut"
                                class="CVAudio1-bg"
                                src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/9cf776446c743f0e7efaa5d85d20a39b_6523050722555489025.png"
                                alt=""
                                draggable="false"
                            />
                        </div>
                        <div class="CVAudio2">
                            <p class="CVAudio2-text" :style="{ fontSize: fsCVtext }">
                                {{ item.jpCV }}
                            </p>
                            <img
                                @click="audioClick"
                                @mouseover="audioOver"
                                @mouseout="audioOut"
                                class="CVAudio2-bg"
                                src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/9cf776446c743f0e7efaa5d85d20a39b_6523050722555489025.png"
                                alt=""
                                draggable="false"
                            />
                        </div>
                    </div>
                    <!-- 角色视频 -->
                    <div
                        class="characterVideo"
                        :style="{
                            top: tcharacterVideo,
                            left: lcharacterVideo,
                            width: wcharacterVideo,
                            height: wcharacterVideo,
                        }"
                    >
                        <img
                            @click="videoClick(item.id)"
                            @mouseover="videoOver"
                            @mouseout="videoOut"
                            class="characterVideoBg"
                            src="https://uploadstatic.mihoyo.com/puzzle/upload/puzzle/2022/08/15/4f95de627d302f84e4256d843f10f444_420562542973104737.png"
                            alt=""
                            draggable="false"
                        />
                    </div>
                    <img class="slideWidth" :src="item.url" alt="" draggable="false" />
                </div>
            </div>
        </div>
        <!-- 前进后退按钮 -->
        <div
            class="leftArrow"
            @click="prevSlide"
            :style="{ top: tArrow, left: lLeftArrow, height: hArrow, width: hArrow }"
        >
            <img
                @mouseover="leftArrowOver"
                @mouseleave="leftArrowLeave"
                class="leftArrowBg"
                :src="leftArrowChangeBg"
                draggable="false"
                alt=""
            />
        </div>
        <div
            class="rightArrow"
            @click="nextSlide"
            :style="{ top: tArrow, left: lRightArrow, height: hArrow, width: hArrow }"
        >
            <img
                @mouseover="rightArrowOver"
                @mouseleave="rightArrowLeave"
                class="rightArrowBg"
                :src="rightArrowChangeBg"
                draggable="false"
                alt=""
            />
        </div>
    </div>

    <!-- 缩略图 -->
    <div
        class="swiper swiper3Thumbs"
        :style="{
            top: tSwiper3Thumbs,
            left: lSwiper3Thumbs,
            width: wSwiper3Thumbs,
            height: hSwiper3Thumbs,
        }"
    >
        <div class="swiper-wrapper">
            <div
                class="swiper-slide wtest pcNoActive"
                v-for="item in p3CharactersThumbs"
                :key="item.id"
            >
                <div class="slideHeight">
                    <img class="slideHeight" :src="item.url" alt="" draggable="false" />
                </div>
            </div>
        </div>
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
.characterVideo {
    position: absolute;
    top: 125px;
    left: 230px;
    width: 43px;
    height: 43px;
    /* background-color: rgba(0, 0, 0, 0.2); */
}

.characterVideoBg {
    width: 100%;
}

.CVAudio {
    position: absolute;
    text-align: center;
    /* background-color: rgba(0, 0, 0, 0.2); */
}

.CVAudio .CVAudio1 {
    position: relative;
    width: 100%;
    height: 50%;
    padding-top: 4%;
    /* background-color: rgba(0, 0, 0, 0.2); */
}
.CVAudio .CVAudio2 {
    position: relative;
    width: 100%;
    height: 50%;
    padding-top: 4%;
}

.CVAudio .CVAudio1-text {
    pointer-events: none;
    position: relative;
    margin: 0;
    font-size: 12px;
    color: white;
    z-index: 1;
}
.CVAudio .CVAudio1-bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    /* z-index: 0; */
}
.CVAudio .CVAudio2-text {
    pointer-events: none;
    position: relative;
    margin: 0;
    font-size: 12px;
    color: white;
    z-index: 1;
}
.CVAudio .CVAudio2-bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    /* z-index: 0; */
}

.swiper3Thumbs {
    position: absolute;
    top: 68%;
    left: 64px;
    width: 150px;
    height: 50px;
    /* background-color: rgba(0, 0, 0, 0.2); */
    z-index: 2;
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

.paginationBullets-active {
    background-color: rgba(255, 0, 0, 0.5);
}

.pcNoActive {
    transform: scale(0.7);
    transition: 0.1s linear;
}

.pcActive {
    transform: scale(1);
    transition: 0.1s linear;
}

.slideHeight {
    height: 100%;
}

.slideWidth {
    width: 100%;
}

.leftArrow {
    position: absolute;
    /* left: 45px; */
    top: 70%;
    width: 30px;
    height: 30px;
    z-index: 2;
    /* background-color: rgba(255, 0, 0, 0.2); */
}

.leftArrow .leftArrowBg {
    height: 100%;
}

.rightArrow {
    position: absolute;
    left: 300px;
    top: 70%;
    width: 30px;
    height: 30px;
    z-index: 2;
    /* background-color: rgba(0, 255, 0, 0.2); */
}

.rightArrow .rightArrowBg {
    height: 100%;
}
</style>
