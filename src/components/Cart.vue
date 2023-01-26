<script setup>
    import CartItem from './CartItem.vue'
    defineProps(['cartItems', 'total'])
    const emit = defineEmits(['increaseItem', 'decreaseItem', 'removeFromCart'])
    const increaseQuantity = id => emit('increaseItem', id)
    const decreaseQuantity = id => emit('decreaseItem', id)
    const handleRemoveFromCart = id => emit('removeFromCart', id)
</script>

<template>
    <div class="cart">
        <h2>Your Cart</h2>
        <div class="cart-items" v-if="cartItems.length">
            <CartItem 
                v-for="item in cartItems" 
                :key="item.id" 
                :cartItem="item"
                @increaseItem="increaseQuantity"
                @decreaseItem="decreaseQuantity"
                @removeFromCart="handleRemoveFromCart"
                />
        </div>
        <div v-else>
            Your cart is empty.
        </div>
        <div class="final-bill" v-if="cartItems.length">
            <div class="sub-total">
                Subtotal: <span>${{ total.subTotal.toFixed(2) }}</span>
            </div>
            <div class="tax">
                Tax: <span>${{ total.tax.toFixed(2) }}</span>
            </div>
            <div class="grand-total">
                Total: <span>${{ total.grandTotal.toFixed(2) }}</span>
            </div>
        </div>
    </div>
</template>

<style lang="less" scoped>
    @import '../less/_colors.less';
    .cart{
        padding: 0 30px;
        padding-bottom: 40px;
        h2{
            margin: 40px 0;
        }
        .final-bill{
            padding-top: 30px;
            div{
                text-align: right;
                font-weight: 700;
                margin-top: 15px;
                display: flex;
                align-items: center;
                justify-content: flex-end;
                gap: 20px;
                span{
                    font-size: 32px;
                    width: 110px;
                }
            }
            .grand-total{
                span{
                    color: @primary;
                }
            }
        }
    }
</style>