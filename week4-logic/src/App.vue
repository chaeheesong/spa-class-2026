<script setup>
import { ref }  from 'vue';

  // 1. 검색 관련 상태 변수
  const searchInput = ref('');
  const searchKeyword = ref('');
  const currentTab = ref('전체');

  // 2. 모달창 상태 변수
  const isModalOpen = ref(false);
  const selectedMovie = ref(null);

  // 3. 영화 배열 데이터(placehold.co 이미지와 평점 포함)
  const movies = ref([
    { id: 1, title: '다크 나이트', genre: '액션', rating: 9.5, poster: 'https://'},
    { id: 2, title: '인터스텔라', genre: 'SF', rating: 8.6, poster: 'https://'},
    { id: 3, title: '어바웃 타임', genre: '로맨스', rating: 7.5, poster: 'https://'},
    { id: 4, title: '인셉션', genre: 'SF', rating: 9.2, poster: 'https://'},
    { id: 5, title: '라라랜드', genre: '로맨스', rating: 6.8, poster: 'https://'},
  ])

  // 4. 모달창 열기 함수
  const openModal = (movie) => {
    selectedMovie.value = movie;
    isModalOpen.value = true;
  };
  const deleteMovie = (targetId) => {
    movies.value = movies.value.filter(movie => movie.id !== targetId);
    isModalOpen.value = false;
  }
  import { computed } from 'vue';

  const filteredMovies = computed(() => {
    return movies.value.filter(movie => {
      const matchKeyword = movie.title.includes(searchKeyword.value);
      const matchTab =
        currentTab.value === '전체' ||
        movie.genre === currentTab.value;

      return matchKeyword && matchTab;
    });
  });
</script>

<template>
  <div>
    <div class="search-area">
      <input 
        type="text"
        v-model="searchInput"
        @keyup.enter="searchKeyword = searchInput"
        placeholder="영화 제목을 입력하세요">
      <button @click="searchKeyword = searchInput">검색</button> 
    </div>
    <div class="movie-grid">
        <div 
          v-for="movie in filteredMovies"
          :key="movie.id"
          class="card"
          @click="openModal(movie)"
        >
          <img :src="movie.poster" alt="포스터" />
          <h3>{{ movie.title }}</h3>
          <p>{{ movie.genre }}</p>
          <p>⭐ {{ movie.rating }}</p>
        </div>
    </div>
    <div v-if="isModalOpen && selectedMovie" class="modal-bg" @click="isModalOpen = false">
      <div class="modal-content" @click.stop>
        <h2 v-if="selectedMovie">{{ selectedMovie.title }} 상세정보</h2>
        <img :src="selectedMovie.poster" alt="포스터" />
        <p>장르: {{ selectedMovie.genre }}</p>
        <p>평점: {{ selectedMovie.rating }}</p>
        <div class="modal-actions">
          <button class="close-btn" @click="isModalOpen = false">닫기</button>
          <button class="delete-btn" @click="deleteMovie(selectedMovie.id)">삭제</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  /*기본 레이아웃 CSS*/
  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
    text-align: center;
    font-family: sans-serif;
  }
  .movie-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 30px;
    justify-content: center;
  }
  .card {
    border: 1px solid #000;
    padding: 20px;
    border-radius: 12px;
    width: 220px;
    text-align: center;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.5);
    transition: 0.3s;
    background: white;
  }
  .card img {
    width: 100%;
    border-radius: 8px;
    margin-bottom: 10px;
    box-shadow: rgba(0,0,0,0.1);
  }
  .card h3 {
    margin: 10px 0 5px 0;
  }
  .search-area input {
    padding: 10px;
    width: 260px;
    border: 1px solid #ccc;
    border-radius: 6px;
    margin-right: 8px;
    font-size: 15px;
  }
  .search-area button {
    padding: 10px 20px;
    background-color: #333;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-weight: bold;
  }
  .tabs button {
    padding: 10px 20px;
    justify-content: 1px solid #ddd;
    background: #f9f9f9;
    cursor: pointer;
    border-radius: 8px;
    transition: 0.2s;
    font-size: 15px;
  }
  .badge-area {
    margin: 15px 0;
    font-weight: bold;
    font-size: 14px;
  }
  .badge {
    padding: 5px 10px;
    border-radius: 20px;
  }
  .badge.master {
    background-color: #f70700;
    color: #000a0b;
  }
  .badge.good {
    background-color: #ae0f7a;
    color: #00030f;
  }
  .badge.soso {
    background-color: #eeeeee;
    color: #816161;
  }
  .detail-btn {
    width: 100%;
    padding: 10px;
    background-color: #4bb633;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
    margin-top: 5px;
  }
  .modal-bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  .modal-content {
    background: white;
    padding: 30px;
    border-radius: 16px;
    width: 320px;
    text-align: center;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  }
  .modal-content img{
    width: 150%;
    border-radius: 8px;
    margin-bottom: 15px;
  }
  .modal-actions {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 25px;
  }
  .modal-actions button {
    padding: 10px 20px;
    border: none;
    border-radius: 0px;
    cursor: pointer;
    font-weight: bold;
  }
  .close-btn {
    background-color: #eeeeee;
    color: #537;
  }
  .delete-btn {
    background-color: #633333;
    color: white;
  }
</style>
