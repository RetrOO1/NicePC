<template>
    <div class="cart_item">
        <div class="product">
            <img :src="path + cart_item.image" alt="computer-parts" width=150 height=150>
            <p class="name" href="#">{{ cart_item.name }}</p>
            <p class="price" href="#">{{ cart_item.price }}₽</p>
            <button @click="deleteItem">
                <img src="@/resoures/delete_button.svg" alt="delete-button">
            </button>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            path: 'https://nicepc-1-s2894705.deta.app/',
        }
    },
    props: {
        cart_item: {
            type: Object,
            required: true,
        }
    },
    methods: {
        async deleteItem() {
            await axios.get(`https://nicepc-1-s2894705.deta.app/cart/delete/${this.cart_item.cart_item_id}`, { headers: { 'Authorization': `Bearer ${localStorage.getItem('token')}` } })
            location.reload()
        }
    }
}
</script>

<style lang="scss">
h2 {
    margin-top: 10px;
    margin-bottom: 20px;
    font-size: 30px;
}

.cart_item {
    display: flex;
    margin: 0 auto;
    width: 80%;
    justify-content: space-between;

    .product {
        display: flex;
        font-size: 20px;
        margin-bottom: 90px;
        width: 100%;
        align-items: center;
        justify-content: space-around;

        .name,
        .price {
            margin-top: 15px;
        }
    }

}
</style>