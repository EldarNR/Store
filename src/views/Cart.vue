<script>
import { defineComponent } from 'vue';
export default defineComponent({
    name: "CartView"
})
</script>

<script setup>
import { productsStore } from '../stores/products';
import { useRouter } from 'vue-router';

const store = productsStore()
const router = useRouter();

const removeFormCart = (id) => {
    store.removeFromCart(id);
}
</script>

<template>
    <button class="button" @click="router.push({ name: 'Catalog' })">Back</button>
    <div v-if="!store.cart.length">
        <p>Empty Cart</p>
    </div>

    <div class="cart-items" v-else>
        <div class="cart-item" v-for="item in store.cart" :key="item.id">
            <div class="item-details">
                <img :src="item.thumbnail" alt="">
                <span>Bran:{{ item.brand }}</span>
                <span>Category:{{ item.category }}</span>
                <span>Price:{{ item.price }}</span>
                <button @click="removeFormCart(item.id)">Remove</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.item-details {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 32px;
    box-shadow: 0 0 17px 6 px #e7e7e7;
    border-radius: 8px;
    padding: 16px;
}

.item-details img {
    width: 20%;
}

.button {
    background-color: #04AA6D;
    /* Green */
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
}
</style>