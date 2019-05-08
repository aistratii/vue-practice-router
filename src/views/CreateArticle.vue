<template>
    <div class="container">
        <div class="row">
            <div class="col-md-8 offset-md-2">
                <div class="card my-5">
                    <div class="card-body">
                        <picture-input 
                            accept="image/jpeg,image/png" 
                            size="10" 
                            button-class="btn btn-danger"
                            @change="onChange">
                        </picture-input>
                        <input type="text" placeholder="Title" class="form-control mb-3">
                        <wysiwyg v-model="content"/>
                        <div class="text-center">
                            <button @click="createArticle" class="btn btn-success mt-3">Create article</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import PictureInput from 'vue-picture-input';
import Axios from 'axios';
import config from "@/config";

export default {
    components: {
        PictureInput
    },
    data() {
        return {
            content: "",
            image: null
        }
    },
    methods: {
        onChange(image){
            this.image = image
        },

        createArticle(){
            const form = new FormData();
            form.append('file', this.image);
            form.append('upload_preset', 'my-preset-name');
            form.append('api_key', '432956187613698');

            Axios.post(`${config.imageUploadUrl}`, form)
            .then(response => {
                console.log(response);
            }).catch(error => {
                console.error(error);
            });
        }
    }
}
</script>

