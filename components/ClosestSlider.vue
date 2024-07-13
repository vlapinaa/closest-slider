<script setup>
import { ref } from 'vue';

const sliderLines = [["Как использовать vue?", "Что это?", "Покажи документацию", "Кто поддерживает Vue?", "Пользовательские директивы"], ["События", "В чем разница между Vue 2 и Vue 3?", "Компоненты жизненного цикла", "Надежен ли Vue?", "Установить"]];

const container = ref(null);

const scrollToRight = () => {
    const elements = container.value.getElementsByClassName("slider__item")
    const containerRect = container.value.getBoundingClientRect();
    let nearestElement = null;
    let minDistance = Infinity;

    [...elements].forEach((element) => {
        const rect = element.getBoundingClientRect();
        const distance = Math.round(rect.left) - Math.round(containerRect.left);

        if (distance <= 0) {
            return;
        }
        if (distance < minDistance) {
            minDistance = distance;
            nearestElement = element;
        }
    });

    if (nearestElement) {
        const newRect = nearestElement.getBoundingClientRect();
        container.value.scrollTo({
            left: Math.round(newRect.left) - Math.round(containerRect.left) + Math.round(container.value.scrollLeft),
            behavior: 'smooth',
        });
    }
};

const scrollToLeft = () => {
    const elements = container.value.getElementsByClassName("slider__item")
    const containerRect = container.value.getBoundingClientRect();
    let nearestElement = null;
    let minDistance = Infinity;

    [...elements].forEach((element) => {
        const rect = element.getBoundingClientRect();
        const distance = Math.round(containerRect.left) - Math.round(rect.left)

        console.log("rect.right", rect.right, "containerRect.right", containerRect.right)

        if (distance <= 0) {
            return;
        }
        if (distance < minDistance) {
            minDistance = distance;
            nearestElement = element;
        }
    });

    if (nearestElement) {
        const newRect = nearestElement.getBoundingClientRect();
        container.value.scrollTo({
            left: Math.round(newRect.left) - Math.round(containerRect.left) + Math.round(container.value.scrollLeft),
            behavior: 'smooth',
        });
    }
};

</script>


<template>
    <div class="slider">
        <button class="slider__btn" @click="scrollToLeft">⟵</button>

        <div class="slider__container" ref="container">
            <div v-for="(line, index) in sliderLines" :key="`${index}-${line.length}`" class="slider__line">
                <div v-for="(item, index) in line" :key="`${index}-${item}`" class="slider__item">
                    {{ item }}
                </div>
            </div>
        </div>

        <button class="slider__btn" @click="scrollToRight">⟶</button>
    </div>
</template>



<style scoped>
.slider {
    display: flex;
    align-items: center;
}

.slider__container {
    display: flex;
    flex-direction: column;
    width: 240px;
    flex-shrink: 0;
    overflow-x: auto;
    scrollbar-width: none;
    margin: 0px 16px;

    @media screen and (min-width: 640px) {
        width: 380px;
        margin: 0px 32px;
    }

    @media screen and (min-width: 1240px) {
        width: 580px;
        margin: 0px 46px;
    }
}

.slider__line {
    display: flex;
}

.slider__line:not(:last-of-type) {
    margin-bottom: 10px;

    @media screen and (min-width: 640px) {
        margin-bottom: 16px;
    }

    @media screen and (min-width: 1240px) {
        margin-bottom: 20px;
    }
}

.slider__item {
    font-size: 12px;
    padding: 4px 8px;
    flex-shrink: 0;
    background-color: white;
    border-radius: 20px;

    @media screen and (min-width: 640px) {
        font-size: 22px;
        padding: 6px 12px;
    }

    @media screen and (min-width: 1240px) {
        font-size: 30px;
        padding: 8px 16px;
    }
}

.slider__item:not(:last-of-type) {
    margin-right: 8px;

    @media screen and (min-width: 640px) {
        margin-right: 10px;
    }

    @media screen and (min-width: 1240px) {
        margin-right: 12px;
    }
}

.slider__item:hover {
    background-color: #bb8add;
}

.slider__btn {
    background-color: white;
    box-shadow: none;
    border: 0;
    font-size: 12px;
    padding: 4px 12px;
    border-radius: 20px;

    @media screen and (min-width: 640px) {
        font-size: 22px;
        padding: 6px 18px;
    }

    @media screen and (min-width: 1240px) {
        font-size: 30px;
        padding: 8px 22px;
    }
}

.slider__btn:hover {
    background-color: #00dc82;
}
</style>