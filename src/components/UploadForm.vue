<template>
<h1> Upload Form </h1>

<div class= "main-container">
    <form 
        method="post" 
        enctype="multipart/form-data"
        @submit.prevent="uploadPhoto" 
        id='uploadForm'
    >
        <div class ="form-field">
            <label for="description"> Description </label>
            <textarea name="description" id ="desc"> </textarea>
        </div>
        <div class ="photo-form">
            <label for="photo"> Photo </label>
            <input type="file" name="photo" id= "photo">
        </div>
    <button type="submit" name="submit" class="btn">Submit</button>
    </form>
</div>
</template>

<script>
export default {
    name: 'UploadForm',
    data() {
            return {
                csrf_token: ''
            }
    },
    created() {
        this.getCsrfToken();
    },

    methods: {
        uploadPhoto() {
            let uploadForm = document.getElementById('uploadForm');
            let form_data = new FormData(uploadForm);
        
            fetch("/api/upload", {
                method: 'POST',
                body: form_data,
                headers: {
                    'X-CSRFToken': this.csrf_token
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
        }, 
        getCsrfToken() {
            let self = this;
            fetch('/api/csrf-token')
                .then((response) => response.json())
                .then((data) => {
                    console.log(data);
                    self.csrf_token = data.csrf_token;
            })
        }
    }
}
</script>

<style>

h1 {
    margin-left: 5%;
}

.main-container{
    padding:  4rem;
}
#label {
    font-weight: bold;
}
#desc {
    width: 50%;
    display:block;
    margin-bottom: 1%;
}
#photo {
    display:block;
    margin-bottom: 1%;
    font-weight:bold;
}

textarea {
    height: 50%;
}

.btn{
    color: #fff;
    background-color: rgb(32, 129, 185);
    border-radius: 5px;
    border: none;
    margin-top: 1em;
    padding: 0.45em 1.7em;
}
</style>