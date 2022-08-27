<script setup>
import { ref, computed, onMounted } from "vue";

import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";

const posts = ref([])
const postXpage = 10
const inicio = ref(0)
const fin = ref(postXpage)
const loading = ref(true)

const favorito = ref('')

const cambiarFavorito = (title) => {
  favorito.value = title
}

const next = () => {
  inicio.value = inicio.value + postXpage
  fin.value = fin.value + postXpage
}

const prev = () => {
  inicio.value += -postXpage
  fin.value += -postXpage
}

// onMounted(() => {
// fetchData();
// })

const fetchData = async () => {
    try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
  } catch (error) {
    console.log(error);
  } finally {
    loading.value = false;
  }
}

fetchData();

const maxLength = computed(() => posts.value.length)

</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mi post Favorito: {{ favorito }}</h2>

    <PaginatePost 
      class="mb-2"
      @next="next"
      @previous="prev"
      :inicio="inicio"
      :fin="fin"
      :maxLength="maxLength"
    />
    
    <BlogPost
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id" 
      :title="post.title"
      :id="post.id"
      :body="post.body"
      :cambiarFavorito="cambiarFavorito"
      class="mb-2"
    >
    </BlogPost>
  </div>
</template>