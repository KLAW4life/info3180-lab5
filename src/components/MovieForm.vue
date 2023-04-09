<template>

    <form id="movieForm" @submit.prevent="saveMovie">

        <div class="form-group mb-3">
            <label for="title" class="form-label">Movie Title</label>
            <input 
                type="text" 
                name="title" 
                class="formcontrol" 
            />
        </div>

        <div class="form-group mb-3">
            <label for="description" class="form-label">Movie Description</label>
            <!-- <textarea v-model="text"></textarea> -->
            <input 
                id="description"
                type="textarea" 
                name="description" 
                class="formcontrol" 
            />
        </div>

        <div class="form-group mb-3">
        <label for="poster" class="form-label">Poster</label>
        <input
            id="poster"
            type="file"
            name="poster"
            accept="image/png, image/jpeg, image/jpg"
            class="formcontrol"
        />
        </div>

        <button class="btn btn-primary" type="submit">Submit</button>

    </form>
    

</template>

<script setup>

import { ref, onMounted } from "vue";
let csrf_token = ref("");

onMounted(() => {
    getCsrfToken();
});


function saveMovie(){

    let movieForm = document.getElementById('movieForm');
    let form_data = new FormData(movieForm);

    fetch("/api/v1/movies", {
        method: 'POST',
        body: form_data,
        headers: {
            'X-CSRFToken': csrf_token.value
        }
    })
        .then(function (response) {
            return response.json();
        })
        .then(function (data) {
            // display a success message
            console.log(data);
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

<!-- Spider-Man: Across the Spider-Verse  -->
<!-- After reuniting with Gwen Stacy, Brooklyn's full-time, friendly neighborhood Spider-Man is catapulted across the Multiverse, 
    where he encounters a team of Spider-People charged with protecting its very existence. 
    However, when the heroes clash on how to handle a new threat, Miles finds himself pitted against the other Spiders. 
    He must soon redefine what it means to be a hero so he can save the people he loves most. -->