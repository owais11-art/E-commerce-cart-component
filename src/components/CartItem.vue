<script setup>
    const props = defineProps(['cartItem'])
    const emit = defineEmits(['increaseItem', 'decreaseItem', 'removeFromCart'])
    const increaseQuantity = id => emit('increaseItem', id)
    const decreaseQuantity = id => emit('decreaseItem', id)
    const handleRemoveFromCart = id => emit('removeFromCart', id)
    const getImage = path => {
        return new URL(path, import.meta.url).href
    }
</script>

<template>
    <div class="cart-item">
        <div class="image">
            <div class="count">{{ cartItem.count }}</div>
            <img :src="getImage(`/src/assets/${cartItem.image}`)" :alt="cartItem.alt"/>
        </div>
        <div class="content">
            <p class="name">{{ cartItem.name }}</p>
            <h5 class="price">${{ cartItem.price }}</h5>
            <div class="dish-info">
                <div class="quantity-wrapper">
                    <div 
                        class="left-chevron chevron" 
                        @click="() => decreaseQuantity(cartItem.id)">
                        <img src="/src/assets/chevron.svg" alt="left-chevron"/>
                    </div>
                    <div class="quantity">{{ cartItem.count }}</div>
                    <div 
                        class="right-chevron chevron"
                        @click="() => increaseQuantity(cartItem.id)">
                        <img src="/src/assets/chevron.svg" alt="left-chevron"/>
                    </div>
                </div>
                <div class="total">
                    ${{ cartItem.total.toFixed(2) }}
                </div>
            </div>
        </div>
        <div 
            class="remove-from-cart" 
            title="Remove from Cart" 
            v-if="!cartItem.count"
            @click="() => handleRemoveFromCart(cartItem.id)"
        >
            &#10005;
        </div>
    </div>
</template>

<style scoped lang="less">
    @import '../less/_colors.less';
    .cart-item{
        display: flex;
        gap: 10px;
        padding-bottom: 30px;
        border-bottom: 1px solid @border-color;
        margin-top: 35px;
        position: relative;
        &:last-child{
            border-width: 5px;
        }
        .image{
            position: relative;
            width: 64px;
            height: 64px;
            img{
                width: 100%;
                height: auto;
            }
            .count{
                display: grid;
                place-items: center;
                font-weight: 700;
                width: 32px;
                height: 32px;
                border-radius: 50%;
                background-color: #000;
                color: #fff;
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
            }
        }
        .content{
            flex-grow: 1;
            .name{
                font-size: 18px;
            }
            .price{
                font-size: 16px;
            }
            .dish-info{
                margin-top: 20px;
                display: flex;
                justify-content: space-between;
                align-items: center;
                width: 100%;
                .quantity-wrapper{
                    display: flex;
                    user-select: none;
                    div{
                        width: 32px;
                        height: 32px;
                        border-radius: 50%;
                        display: grid;
                        place-items: center;
                    }
                    .chevron{
                        background-color: @primary;
                        cursor: pointer;
                        img{
                            width: 13px;
                            height: 13px;
                        }
                    }
                    .quantity{
                        font-weight: 700;
                    }
                    .right-chevron{
                        transform: rotate(180deg);
                    }
                }
                .total{
                    font-weight: 700;
                    font-size: 32px;
                }
            }
        }
        .remove-from-cart{
            position: absolute;
            right: -10px;
            top: -25px;
            color: red;
            font-weight: bolder;
            width: 25px;
            height: 25px;
            border-radius: 50%;
            display: grid;
            place-items: center;
            cursor: pointer;
            transition: all .2s;
            background-color: @pattens-blue;
            &:hover{
                background-color: red;
                color: @pattens-blue;
            }
        }
    }
</style>