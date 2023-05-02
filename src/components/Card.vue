<script setup>
defineProps({
  listMovies: {
    type: Array
  },
  handleModal: {
    type: Function
  }
})
</script>

<template>
    <div v-for="{show: {id, name, image, network, rating, premiered, language}} in listMovies" class="card">
      <img v-if="image" :src="image.original" alt="" srcset="" class="card__img">
      <img v-else src="../assets/movie.jpg" alt="" srcset="" class="card__img">
      <div class="card__box">
        <div class="card__top">
          <div>{{language}}</div>
          <div class="card__rating">
            <img src="../assets/stars.png" alt="" srcset="" class="card__star">
            <span>{{ rating.average ? rating.average : '-' }}</span>
          </div>
        </div>

        <div class="card__bottom">
          <h1 class="card__title">{{name}}</h1>
          <div class="card__desc">
            <div class="card__region">
              <h2>{{network ? network.country.name : '-'}}</h2> |
              <h2>{{ premiered ? premiered.split('-')[0] : '-' }}</h2>
            </div>
            <button class="card__cta" @click="() => {handleModal(id)}">See Detail</button>
          </div>
        </div>
      </div>
    </div>
</template>