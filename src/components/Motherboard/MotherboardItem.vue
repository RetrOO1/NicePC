<template>
    <div>
        <div class="cpu_item">
            <div class="left-box">
                <div>
                    <img class="cpu_picture" :src="path+motherboard_item.image" alt="">
                </div>
                <div>
                    <div class="cpu_name">
                        <a href="">
                            {{ motherboard_item.name }}
                        </a>
                    </div>
                    <div class="css_line">
                    </div>
                    <ul class="cpu_specifications">
                        <li>Форм-фактор: {{ motherboard_item.param1 }}</li>
                        <li>Сокет: {{ motherboard_item.param2 }}</li>
                        <li>Память: {{ motherboard_item.param3 }}</li>
                        <li>Слоты: {{ motherboard_item.param4 }}</li>
                        <li>Разъемы: {{ motherboard_item.param5 }}</li>
                        <li>Сеть: {{ motherboard_item.param6 }}</li>
                    </ul>
                </div>
            </div>
            <div>
                <p class="cpu_price">{{ motherboard_item.price }}₽</p>

                <div v-if="!isInCart" class="buy_button">
                    <button @click="addToCart">Купить</button>
                </div>
                <div v-if="isInCart" class="buy_button">
                    <a href="cart">К корзине</a>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
            path: 'https://nicepc-1-s2894705.deta.app/',
            isInCart: false
        }  
    },
    props: {
        motherboard_item: {
            type: Object,
            required: true,
        }
    },
    methods: {  
        async addToCart() {
            await axios.post('https://nicepc-1-s2894705.deta.app/cart/', {'product_id': this.motherboard_item.id}, {headers: {'Authorization': `Bearer ${localStorage.getItem('token')}`}})
            .then((res) => {
                if (res.status === 200) {
                    this.isInCart = !this.isInCart
                }
            })
        }
    }
}
</script>

<style>

.cpu_item {
    display: flex;
    justify-content: space-between;
    max-width: 1500px;
    margin: 0 auto;
    margin-bottom: 100px;
}

.left-box {
    display: flex;
}

.cpu_picture {
    width: 200px;
    height: 200px;
    margin-right: 50px;
}

.cpu_name {
    margin-bottom: 20px;
    font-size: 20px;
}

.css_line{
    margin-bottom: 40px;
    width: 400px;
    height: 1px;
    background-color: rgb(202, 202, 202);
}

.cpu_specifications li {
    line-height: 20px;
}

.cpu_price {
    font-size: 25px;
    margin-bottom: 20px;
}

.buy_button {
    border-radius: 6px;
    border: 1px solid transparent;
    text-align: center;
    background-color: rgba(128, 0, 128, 0.5);
    width: 120px;
    height: 50px;
}

.buy_button a {
    font-size: 16px;
    line-height: 50px;
}

.buy_button button {
   padding-top: 15px;
   font-size: 16px;
}
</style>