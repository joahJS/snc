<template>
    <div id="mainSlider">
        <swiper id="mainSliderContainer" v-bind="mainSliderSettings">
            <swiper-slide v-for="slide in mainSlides" :key="slide.uniqueKey" class="mainslide-container">
                <div class="mainslide-overlay">
                    <h2 class="mainslide-title">{{ slide.slideTitle }}</h2>
                    <p class="mainslide-subtitle">{{ slide.subText }}</p>
                </div>

                <div class="mainslide-visual">
                    <video v-if="slide.visType === 'video'" class="visual-source" muted autoplay loop>
                        <source :src="slide.visSrc" type="video/mp4">
                    </video>

                    <img v-else-if="slide.visType === 'image'" class="visual-source" :src="slide.visSrc !== '' ? slide.visSrc : '/images/not-exist.png'" alt="">
                </div>
            </swiper-slide>
        </swiper>

        <button type="button" id="btnSliderPrev">
            <i class="ri-arrow-left-s-line"></i>
        </button>

        <button type="button" id="btnSliderNext">
            <i class="ri-arrow-right-s-line"></i>
        </button>

        <div id="scrollIndicator">

        </div>
    </div>
</template> <!-- Template Ends -->

<script setup>
    import { Swiper, SwiperSlide } from 'swiper/vue'
    import { Autoplay, Navigation, A11y, EffectFade } from 'swiper'

    import 'swiper/scss'
    import 'swiper/scss/autoplay'
    import 'swiper/scss/navigation'
    import 'swiper/scss/a11y'
    import 'swiper/scss/effect-fade'

    // const upperPath = '/temp/'

    const mainSliderSettings = {
        modules: [ Autoplay, Navigation, A11y, EffectFade ],
        slidesPerView: 1,
        autoplay: {
            delay: 7500,
            disableOnInteraction: false
        },
        speed: 500,
        navigation: {
            prevEl: '#btnSliderPrev',
            nextEl: '#btnSliderNext'
        },
        effect: 'fade',
        onSlideChange: () => {
            // 슬라이드 정보
        }
    }

    const mainSlides = ref([
        {
            uniqueKey: 0,
            slideTitle: '고객의 꿈과 행복을 함께 만들어가는 기업',
            subText: 'SNC는 앞서가는 기술력과 노하우로 현재 업계 선두를 달리고 있습니다.',
            visType: 'image',
            visSrc: '/assets/images/snc_main/img_load01.jpg'
        },
        {
            uniqueKey: 1,
            slideTitle: '고객의 꿈과 행복을 함께 만들어가는 기업',
            subText: 'SNC는 지난 수년간의 노력과 시행착오를 거치며 고객님들과 더불어 성장하였습니다.',
            visType: 'image',
            visSrc: `/assets/images/snc_main/img_load02.jpg`
        },
    ])
</script> <!-- Logic Ends -->

<style lang="scss" scoped>
    #mainSlider {
        position: relative;
    }

    #btnSliderPrev,
    #btnSliderNext {
        position: absolute;
        inset: 0;
        margin-block: auto;
        width: fit-content;
        height: fit-content;
        color: rgb(var(--main));
        font-size: 75px;
        z-index: 2;

        &.swiper-button-disabled {
            color: rgb(var(--white));
            opacity: .25;
        }
    }

    #btnSliderPrev {
        margin-inline-end: auto;
    }

    #btnSliderNext {
        margin-inline-start: auto;
    }

    #mainSliderContainer {
        isolation: isolate;
    }

    .mainslide-container {
        height: 100vh;

        &.swiper-slide-active {
            .mainslide-title,
            .mainslide-subtitle {
                transform: none;
                opacity: 1;
                transition: all 1s;
            }
        }
    }

    .mainslide-overlay {
        display: flex;
        flex-flow: column nowrap;
        justify-content: center;
        align-items: center;
        gap: 25px;
        position: absolute;
        inset: 0;
        color: rgb(var(--white)); // temp
        isolation: isolate;
        user-select: none;
    }

    .mainslide-title {
        color: transparent;
        // font-size: max(5vmax, min(40px, 5vh));
        font-size: 8vh;
        font-weight: 900;
        -webkit-text-fill-color: transparent;
        -webkit-text-stroke-width: 2px;
        -webkit-text-stroke-color: rgb(var(--white));
        word-break: keep-all;
        transform: translateY(40px);
        opacity: 0;
    }

    .mainslide-subtitle {
        color: rgb(var(--white));
        // font-size: max(2.5vmax, min(20px, 2.5vh));
        font-size: 2rem;
        font-weight: 900;
        word-break: keep-all;
        transform: translateY(-40px);
        opacity: 0;
        transition-delay: .5s;
    }

    .mainslide-visual {
        position: absolute;
        inset: 0;
        z-index: -1;
    }

    .visual-source {
        display: block;
        width: 100%;
        height: 100%;
        object-fit: cover;
        filter: brightness(.45);
    }

    #scrollIndicator {
        position: absolute;
        inset-inline: 0;
        bottom: 30px;
        margin-inline: auto;
        width: 30px;
        height: 50px;
        border: 2px solid rgba(var(--white), .75);
        border-radius: 999px;
        z-index: 2;

        &::before {
            display: block;
            position: absolute;
            inset-inline: 0;
            top: 5px;
            margin-inline: auto;
            width: 4px;
            height: 7px;
            border-radius: 999px;
            background-color: rgba(var(--white), .75);
            animation: wheelDown 1s ease-in-out infinite;
            content: '';
        }
    }

    @keyframes wheelDown {
        0% {
            top: 5px;
            opacity: 0;
        }

        10% {
            opacity: .15;
        }

        50% {
            opacity: 1;
        }

        90% {
            opacity: .15;
        }

        100% {
            top: 15px;
            opacity: 0;
        }
    }

    @media screen and (max-width: 960px) {
        #btnSliderPrev,
        #btnSliderNext {
            top: auto;
        }

        .mainslide-overlay {
            align-items: flex-start;
            padding-inline: 30px;
            line-height: 1.2;
        }

        .mainslide-title {
            -webkit-text-stroke-width: .5px;
        }

        .mainslide-subtitle {
            font-size: 18px;
            font-weight: 500;
            text-align: start;
        }
    }
</style> <!-- Stylesheet Ends -->