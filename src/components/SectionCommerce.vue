<template>
    <div class="container">
        <div v-if="product" v-bind:class="isFemale ? 'female-bg': 'male-bg'">
            <div class="card">
                <img class="product-img" v-bind:src="product.image"/>
                <h3 v-bind:class="isFemale ? 'product-title-female': 'product-title-male'">
                    {{product.title}}
                </h3>
                <p class="product-type">{{product.category}}</p>
                <p class="product-rating">{{product.rating.rate}}/5</p>
                <RatingCommerce :rating="roundedRating" :gender="productGender" />
                <div class="divider-top"></div>
                <p class="product-description">
                    {{truncatedDescription}}
                </p>
                <p v-bind:class="isFemale ? 'product-price-female': 'product-price-male'">${{product.price}}</p>
                <div class="button-layout">
                    <button v-bind:class="isFemale ? 'button-buy-female': 'button-buy-male'">Buy Now</button>
                    <button @click="nextProduct" v-bind:class="isFemale ? 'button-next-female': 'button-next-male'">Next Product</button>
                </div>
                <div class="divider-bottom"></div>
            </div>
        </div>
        <div v-else class="unavail-bg">
            <div class="card" style="text-align: center;">
                <img class="unavail-img" src="https://res.cloudinary.com/diyb3la39/image/upload/v1679384619/Random/sad-face_chjrhs.png"/>
                <div class="unavail-container">
                    <p>This Product is Unable to shown</p>
                    <button @click="nextProduct" class="unavail-button">Next Product</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import RatingCommerce from './RatingCommerce.vue'
    import axios from 'axios';

    export default {
        components: {
            RatingCommerce
        },
        data() {
            return {
                isFemale:false,
                product: null,
                productId: 1,
                maxProductId: 20,
            }
        },
        mounted() {
            this.fetchProduct();
        },
        watch: {
            product: {
                handler(newProduct) {
                    this.isFemale = newProduct.category.includes("wo");
                },
                immediate: true,
            },
        },
        methods: {
            fetchProduct() {
            axios.get(`https://fakestoreapi.com/products/${this.productId}`)
                .then(response => {
                    if (response.data.category === "men's clothing" || response.data.category === "women's clothing") {
                        this.product = response.data;
                    }else {
                        this.product = null;
                    }
                })
                .catch(error => {
                    console.log(error);
                });
            },

            nextProduct() {
                if (this.productId < this.maxProductId) {
                    this.productId++;
                } else {
                    this.productId = 1;
                }
                this.fetchProduct();
            },
        },
        computed: {
            productGender() {
                if (this.product.category.includes("wo")) {
                    return 'female';
                } else {
                    return 'male';
                }
            },
            truncatedDescription() {
                const words = this.product.description.split(' ');
                if (words.length > 50) {
                    return words.slice(0, 50).join(' ') + '...';
                } else {
                    return this.product.description;
                }
            },
            roundedRating() {
                return Math.floor(this.product.rating.rate);
            }
        }
    }
</script>
<style scoped>
    @import '../assets/style/page.css'
</style>
