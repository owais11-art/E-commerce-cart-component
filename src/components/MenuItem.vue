<script setup>
    import { computed, inject, ref } from 'vue'
    const props = defineProps(['menuItem'])
    const emit = defineEmits(['addToCart'])
    const cart = inject('cart-items')
    const isInCart = computed(() => cart.value.some(item => item.id === props.menuItem.id))
    const cartText = computed(() => isInCart.value ? 'In Cart' : 'Add to Cart')
    const generateRandomLightColor = () => {
        const getRandomNumber = (max = 255) => Math.floor(Math.random() * max)
        const r = getRandomNumber()
        const g = getRandomNumber()
        const b = getRandomNumber()
        const a = 0.2
        return `rgba(${r}, ${g}, ${b}, ${a})`
    }
    const bgColor = ref(generateRandomLightColor())
    const handleClick = () => emit('addToCart', props.menuItem)
</script>

<template>
    <div class="menu-item">
        <div class="image">
            <img :src="menuItem.image" :alt="menuItem.alt">
        </div>
        <div class="content">
            <p class="name">{{ menuItem.name }}</p>
            <h3 class="price">${{ menuItem.price }}</h3>
            <button @click="handleClick">
                <img src="/src/assets/check.svg" alt="check-mark" v-if="isInCart">
                {{ cartText }}
            </button>
        </div>
    </div>
</template>

<style scoped lang="less">
    @import '../less/_colors.less';
    .menu-item{
        position: relative;
        background-color: v-bind('bgColor');
        display: flex;
        gap: 10px;
        border-radius: 6px 0 0 6px;
        padding-bottom: 10px;
        .image{
            width: 148px;
            height: 148px;
            margin-top: -22px;
            margin-left: -20px;
            img{
                width: 100%;
                height: auto;
            }
        }
        .content{
            padding-top: 20px;
            .name{
                font-size: 18px;
            }
            .price{
                font-size: 32px;
            }
            button{
                height: 24px;
                width: 94px;
                border: none;
                border-radius: 50px;
                background: v-bind('isInCart ? "#000" : "#6B00F5"');
                color: @white-ice;
                font-weight: 700;
                position: absolute;
                bottom: -12px;
                cursor: pointer;
                display: flex;
                align-items: center;
                justify-content: center;
                gap: 5px;
                img{
                    width: 16px;
                    height: 16px;
                }
            }
        }
    }
</style>