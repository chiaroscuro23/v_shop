<template>
    <div class="cart">
        <router-link :to="{name: 'catalog'}">
            <div class="catalog__link_to_cart">Back to Catalog</div>
        </router-link>
        <h1>Cart</h1>
        <p v-if="!cart_data.length">There are no products in cart...</p>
    <v-cart-item
        v-for="(item,index) in cart_data"
        :key="item.article"
        :cart_item_data="item"
        @deleteFromCart="deleteFromCart(index)"
        @increment="increment(index)"
        @decrement="decrement(index)"
    />
        <div class="cart__total">
            <p>Products({{cartQuantity}} items): {{cartTotalCost}} &#8364;</p>
            <p>Delivery Charges: Free</p>
            <p>Total: {{cartTotalCost}} &#8364;</p>
            <div ref="paypal"></div>
        </div>
    </div>
</template>

<script>
    import vCartItem from './v-cart-item'
    import {mapActions} from 'vuex'

    export default {
        name: "v-cart",
        components: {
            vCartItem
        },
        props: {
            cart_data: {
                type: Array,
                default(){
                    return []
                }
            }
        },
        data(){
            return {}
        },
        mounted(){
            {
                const script = document.createElement("script");
                script.src =
                    "https://www.paypal.com/sdk/js?client-id=AQFcIMQLtQVorUt09-S4JnAJRm65JwMOxFUWH28S4Xzn1-00A_hm49UCnUbKiupGf5cgB8yodIQaThtm";
                script.addEventListener("load", this.setLoaded);
                document.body.appendChild(script);
            }
        },
        computed: {
            cartTotalCost() {
                return this.cart_data.map((item) => item.price * item.quantity).reduce((sum, el) => sum + el,0)

            },
            // eslint-disable-next-line vue/return-in-computed-property
            cartQuantity() {
                return this.cart_data.reduce((sum, el) => sum + el.quantity,0)
            },
        },
        methods: {

            ...mapActions([
                'DELETE_FROM_CART',
                'DECREMENT_CART_ITEM',
                'INCREMENT_CART_ITEM'
            ]),
            decrement(index) {
                this.DECREMENT_CART_ITEM(index);
            },
            increment(index) {
                this.INCREMENT_CART_ITEM(index);
            },
            deleteFromCart(index) {
                this.DELETE_FROM_CART(index)
            },
            setLoaded: function() {
                window.paypal
                    .Buttons({
                        createOrder: (data, actions) => {
                            return actions.order.create({
                                purchase_units: [
                                    {
                                        amount: {
                                            currency_code: "USD",
                                            value: this.cartTotalCost
                                        }
                                    }
                                ]
                            });
                        },
                    })
                    .render(this.$refs.paypal);
            },
            },
    }
</script>

<style lang="scss">
    .cart{
        margin-bottom: 100px;
        &__total{
            position: fixed;
            right: 125px;
            bottom: 200px;
            box-shadow: 0 0 8px 0 #e0e0e0;
            padding: $padding*2;
            margin-bottom: $margin*2;
            box-shadow: 0 0 8px 0 #e0e0e0;
            padding: $padding*2;
        }
        .total__name{
            margin-right: $margin*2;
        }
    }
</style>