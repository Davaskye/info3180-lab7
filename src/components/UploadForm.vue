<template>
    <form @submit.prevent="uploadPhoto" id = 'uploadForm'>
    <h1>Upload Form</h1>

    <div class="form-field">
        <label for="description">Description</label>
        <textarea name="description" id="description"></textarea>
    </div>

    <div class="form-field">
        <label for="photo">Photo Upload</label>
        <input name="photo" id="photo" type="file">
    </div>

    <button type="submit">Submit</button>

    </form>
</template>

<script>
export default {
    data() {
        return {
            csrf_token: ''
        };
    },
    created() {
        this.getCsrfToken();

    },
    methods: {
        uploadPhoto() {
            let uploadForm = document.getElementById('uploadForm');
            let form_data = new FormData(uploadForm)
            let self = this;
            fetch('/api/upload', {
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
};
</script>

<style>

.form-field{
    margin: 1rem 0;
}

#description{
    width:50%;
}


button{
    border:none;
    color:#ffff;
    background-color: #2861fd;
    border-radius: 3px;
}

.form-field input, .form-field textarea{
    display: block;
    border: 1px solid #cccccc;
}
</style>