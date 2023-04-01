<template>
    <div class="rating" :class="gender">
        <span v-for="(circle, index) in circles" :key="index" :class="{ filled: index < rating }"></span>
    </div>
</template>

<script>
    export default {
        name: 'RatingCommerce',
        props: {
            rating: {
                type: Number,
                required: true,
                validator: value => value >= 0 && value <= 5
            },
            gender: {
                type: String,
                required: true,
                validator: value => ['male', 'female'].includes(value)
            }
        },
        computed: {
            circles() {
                return Array.from({ length: 5 }, (_, i) => i < this.rating)
            }
        }
    }
</script>

<style scoped>
    @import '../assets/style/rating.css'
</style>