<script setup>
  import { ref }  from 'vue';
  import MovieCard from './components/MovieCard.vue';
  const movies = ref([
    { id: 1, title: '인셉션', rating: 9.5, likes: 0, poster: 'https://picsum.photos/seed/inception/300/450' },
    { id: 2, title: '어바웃', rating: 9.2, likes: 0, poster: 'https://picsum.photos/seed/abouttime/300/450' },
    { id: 3, title: '다크 나이트', rating: 9.2, likes: 0, poster: 'https://picsum.photos/seed/darkknight/300/450' },
    { id: 4, title: '기생충', rating: 8.9, likes: 0, poster: 'https://picsum.photos/seed/parasite/300/450' },
  ]);

  const handleLikeMovie = (id) => {
    const movie = movies.value.find(m => m.id === id);
    if (movie) movie.likes++;
  };

  const handleDeleteMovie = (id) => {
    movies.value = movies.value.filter(m => m.id !== id);
  };
</script>

<template>
  <div class="container">
    <h2>상호작용이 추가된 영화 리스트 (Props & Emit)</h2>
    <main class="movie-grid">
      <MovieCard 
        v-for="m in movies" 
        :key="m.id" 
        :movie="m" 
        @like-movie="handleLikeMovie" 
        @delete-movie="handleDeleteMovie"
      />
    </main>
  </div>
</template>

<style scoped>
  .container { 
    padding: 40px; 
    font-family: sans-serif; 
    max-width: 1000px; 
    margin: 0 auto; 
    background-color: #f8f9fa;
    min-height: 100vh;
  }
  h2 { 
    text-align: center; 
    margin-bottom: 40px; 
    color: #34495e;
    font-weight: 800;
  }
  .movie-grid { 
    display: grid; 
    grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); 
    gap: 25px; 
  }
</style>