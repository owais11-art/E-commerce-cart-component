<script setup>
    import MenuItem from './MenuItem.vue'
    defineProps(['menuItems'])
    const emit = defineEmits(['addToCart'])
    const handleAddEmit = newItem => emit('addToCart', newItem)
</script>

<template>
    <div class="menu">
        <h2>To Go Menus</h2>
        <div class="menu-items">
            <MenuItem 
                v-for="item in menuItems" 
                :key="item.id" 
                :menuItem="item"
                @addToCart="handleAddEmit"
                />
        </div>
    </div>
</template>

<style lang="less" scoped>
    .menu{
        padding-left: 30px;
        padding-bottom: 40px;
        h2{
            margin: 40px 0;
        }
        .menu-items{
            display: flex;
            flex-direction: column;
            gap: 30px;
        }
    }
</style>