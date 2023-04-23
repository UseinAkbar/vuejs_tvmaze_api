<script setup>
import { watch } from 'vue';
const props = defineProps({
  movie: {
    type: Object
  },
  isClicked: {
    type: Boolean
  }
})

console.log(props.movie);
const {id, name, image, network, rating, premiered, summary, language, genres} = props.movie

</script>

<template>
    <div class="modal" :class="isClicked ? 'popup' : null">
        <div class="modal__left">
            <h1 class="modal__title">{{name}}</h1>
            <div class="modal__region">
                <h2>{{network ? network.country.name : '-'}}</h2> |
                <h2>{{ premiered ? premiered.split('-')[0] : '-' }}</h2> |
                <div class="modal__rating">
                    <img src="../assets/stars.png" alt="" srcset="" class="modal__star">
                    <span>{{ rating.average ? rating.average : '-' }}</span>
                </div>
            </div>
            <div class="modal__genres">
                <span v-for="item in genres">{{item}}</span>
            </div>
            <p class="modal__sinopsis">{{summary}}</p>
        </div>
        <div class="modal__right">
            <img :src="image.original" alt="">
        </div>
    </div>
</template>