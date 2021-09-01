<template>
    <div class="cart-item">
        <img class="cart-item__image" :src="require('../assets/images/'+ cart_item_data.image)" alt=""/>
        <div class="cart-item__info">
            <p>{{cart_item_data.name}}</p>
            <p>{{cart_item_data.price}}</p>
            <p>{{cart_item_data.article}}</p>
        </div>
        <div class="cart-item__quantity">
            <p>Qty:</p>
            <span>
            <span   class="quantity__btn"
                    @click="decrementItem">-</span>
            {{cart_item_data.quantity}}
                   <span   class="quantity__btn"
                           @click="incrementItem">+</span>
            </span>
        </div>

        <button class="btn" @click="deleteFromCart">Delete</button>
    </div>
</template>

<script>

export default {
    name: "v-cart-item",
    props: {
        cart_item_data: {
            type: Object,
            default(){
                return {}
            }
        }
    },
    data(){
        return {}
    },
    mounted(){

    },
    methods: {
        decrementItem(){
            this.$emit('decrement')
        },
        incrementItem(){
            this.$emit('increment')
        },
        deleteFromCart(){
            this.$emit('deleteFromCart')
        }
    }
}
</script>

<style lang="scss">
    .cart-item{
        position: relative;
        right: 150px;
        display: flex;
        flex-wrap: nowrap;
        justify-content: space-between;
        align-items: center;
        box-shadow: 0 0 8px 0 #e0e0e0;
        padding: $padding*2;
        margin-bottom: $margin*2;
        &__image{
            max-width: 100px;
        }
        .quantity__btn{
            cursor: pointer;
        }
    }
</style>