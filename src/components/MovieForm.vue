<template>

    <form id="movieForm" @submit.prevent="saveMovie">

        <div v-if="errorMessage" class="alert alert-danger">
        <ul>
            <li v-for="error in errorMessage">{{ error }}</li>
        </ul>
        </div>

        <div v-if="successMessage" class="alert alert-success">{{ successMessage }}</div>

        <div class="form-group mb-3">
            <label for="title" class="form-label" >Movie Title</label>
            <input 
                id="title"    
                type="text" 
                name="title" 
                class="formcontrol" 
                placeholder="Enter Movie Title"
            />
        </div>

        <div class="form-group mb-3">
            <label for="description" class="form-label">Movie Description</label>
            <textarea 
                v-model="text" 
                placeholder="Enter Movie description"
                id="description"
                name="description" 
                class="formcontrol" 
            >
            </textarea>
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
let errorMessage = ref("");
let successMessage = ref("");

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
            
            if ("errors" in data){
                errorMessage.value = [...data.errors];
            } else {
                successMessage.value = "Movie Added Successfully";
                resetFormFields();
            }
    
            console.log(data);
        })
        .catch(function (error) {
            console.log(error);
        });


}

function resetFormFields(){
    errorMessage.value = "";
    title.value = "";
    description.value = "";
    poster.value = "";
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

<style>
.movie-form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

label {
  display: flex;
  flex-direction: column;
  margin: 10px;
  font-size: large;
}

input[type="text"],
textarea {
  width: 100%;
  padding: 0.5rem;
  font-size: 1rem;
  border-radius: 10px;
  border: 1px solid #ccc;
}

textarea {
  height: 150px;
}

.poster-label {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.poster-input {
  margin-bottom: 0.5rem;
}

.poster-preview {
  font-size: 0.8rem;
  color: #666;
}

.submit-button:hover {
  background-color: #0069d9;
}
</style>