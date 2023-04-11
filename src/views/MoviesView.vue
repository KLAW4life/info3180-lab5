
<template>

    <h1 class="title">Movies</h1>
    <div class="container">
      <div class="movie-card" v-for="movie in movies">
        <div class="card mb-3" >
        <div class="row g-0">
          <div class="col-md-5">
            <img :src=movie.poster id="movie-posters" class="img-fluid rounded-start" alt="Movie Poster Image">
          </div>
          <div class="col-md-7" >
            <div class="card-body">
              <h5 class="card-title">{{movie.title}}</h5>
              <p class="card-text">{{movie.description}}</p>
            </div>
          </div>
        </div>
      </div>
      </div>
    </div>
    
</template>
    
<style>

.container{
    display: grid;
    grid-template-columns: auto auto;
    grid-gap: 10px;
}

.title{
    padding-left: 270px;
}

.card{
    max-width: 500px;
}

#movie-posters{
    height: 300px;
    width: 200px;
    object-fit: cover;
}
</style>


<script setup>

import { ref, onMounted } from "vue";

let movies = ref([]);
let csrf_token = ref("");

onMounted(() => {
    getCsrfToken();
    fetchMovies();
});

function fetchMovies(){


    fetch("/api/v1/movies", {
        method: 'GET',
        headers: {
            'X-CSRFToken': csrf_token.value
        }
    })
        .then(function (response) {
            return response.json();
        })
        .then(function (data) {

            movies.value = data.movies;
            // console.log(data);
        })
        .catch(function (error) {
            console.log(error);
        });

}

function getCsrfToken() {

    fetch('/api/v1/csrf-token')
        .then((response) => response.json())
        .then((data) => {
            console.log(data);
            csrf_token.value = data.csrf_token;
        })
}

</script>