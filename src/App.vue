<script setup>
  import { computed, onMounted, ref } from 'vue';
  import BlogPost from './components/BlogPost.vue';
  import PaginatePost from './components/PaginatePost.vue';
  import LoadingSpinner from './components/LoadingSpinner.vue';

  const posts = ref([]);
  const loading = ref(true)

  const postXpage = 10;
  const inicio = ref(0);
  const fin = ref(postXpage);

  const favorito = ref('');

  const cambiarFavorito = (title) => {
    favorito.value = title
  };

  const next = () => {
    inicio.value = inicio.value + postXpage;
    fin.value = fin.value + postXpage;
  };

  const prev = () => {
    inicio.value = inicio.value - postXpage;
    fin.value = fin.value - postXpage;
  }

  const totalPosts = computed(() => posts.value.length)

  const fetchData = async() => {
    try {
      const res = await fetch('https://jsonplaceholder.typicode.com/posts');
      posts.value = await res.json();
    } catch (error) {
      console.log(error)
    } finally {
      loading.value = false;
    }
  };
  

  onMounted(() => {
    fetchData();
  });
</script>

<template>

  <LoadingSpinner v-if="loading"/>

  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mis Post Favoritos: {{ favorito }}</h2>

    <PaginatePost 
      class="mb-2"
      :inicio="inicio"
      :fin="fin"
      :totalPosts="totalPosts"
      @next="next"
      @prev="prev"
    />

    <BlogPost 
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id"
      :title="post.title"
      :id="post.id"
      :body="post.body"
      @cambiarFavoritoNombre="cambiarFavorito"
    />
    
  </div>
</template>