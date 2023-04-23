<script setup>
  import { ref, watch } from 'vue';
  import Navigation from './components/Navigation.vue';
  import Card from './components/Card.vue';
  import Footer from './components/Footer.vue';
  import Modal from './components/Modal.vue';

  const isLoad = ref(true)
  const listMovies = ref([])
  const isClicked = ref(false)
  const selectedMovies = ref({id: '', name: '', image: '', network: '', rating: '', premiered: '', summary: '', language: '', genres: ''})

  async function getData() {
        const data = await fetch("https://api.tvmaze.com/search/shows?q=girls")
        .then(response => response.json())
        .then(res => res)
    
        listMovies.value = data;
        isLoad.value = false
  }

  const handleModal = (selectedId) => {
    const {show} = listMovies.value.find(item => item.show.id === selectedId)
    selectedMovies.value = {...show}
    document.querySelector('body').classList.toggle('fixBody')
    isClicked.value = true
  }

  const handleClose = () => {
    isClicked.value = false
    document.querySelector('body').classList.toggle('fixBody')
  }

  getData()

</script>

<template>
  <Navigation />
  <div class="overlay" :class="isClicked ? 'popup' : null"></div>
  <div class="modal" :class="isClicked ? 'popup' : null">
    <div class="modal__left">
        <h1 class="modal__title">{{selectedMovies.name}}</h1>
        <div class="modal__region">
          <h2>{{selectedMovies.network ? selectedMovies.network.country.name : '-'}}</h2>
          <span class="modal__line">|</span>
          <h2>{{selectedMovies.language}}</h2>
          <span class="modal__line">|</span>
          <h2>{{ selectedMovies.premiered ? selectedMovies.premiered.split('-')[0] : '-' }}</h2>
          <span class="modal__line">|</span>
          <div class="modal__rating">
            <img src="./assets/stars.png" alt="" srcset="" class="modal__star">
            <span>{{ selectedMovies.rating.average ? selectedMovies.rating.average : '-' }}</span>
          </div>
        </div>
        <div class="modal__genres">
            <span v-for="item in selectedMovies.genres">{{item}}</span>
        </div>
        <p class="modal__sinopsis">{{selectedMovies.summary}}</p>
        <a v-if="selectedMovies.officialSite" :href="selectedMovies.officialSite" class="modal__link" target="_blank">
          <img src="./assets/link.svg" alt="">
          Official Site</a>
    </div>
    <div class="modal__right">
        <div class="modal__close" @click="handleClose">
          <img src="./assets/close.svg" alt="">
        </div>
        <img :src="selectedMovies.image.original" alt="">
    </div>
  </div>
  <!-- <Modal :movie="selectedMovies" :isClicked="isClicked" /> -->
  <main>
    <div class="loader" v-if="isLoad">
      <lord-icon
          src="https://cdn.lordicon.com/dpinvufc.json"
          trigger="loop"
          colors="primary:#C2F640"
          class="loader-lottie">
      </lord-icon>
    </div>
    <Card :listMovies="listMovies" :handleModal="handleModal" v-if="!isLoad" />
  </main>
  <div class="gutter"></div>
  <Footer />
</template>