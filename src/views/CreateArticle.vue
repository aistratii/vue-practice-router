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
                        <select class="form-control my-3" v-model="category">
                            <option>Select a category</option>
                            <option v-for="category in categories" :value="category.id" :key="category.id">{{category.name}}</option>
                        </select>
                        <input type="text" v-model="title" placeholder="Title" class="form-control my-3">
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
    beforeRouteEnter(to, from, next){
        if(!localStorage.getItem('auth')){
            return next({path: "/"});
        }

        next();
    },
    mounted() {
        this.getCategories();
    },
    components: {
        PictureInput
    },
    data() {
        return {
            content: "",
            image: null,
            categories: [],
            category:"",
            title: ""
        }
    },
    methods: {
        onChange(image){
            this.image = image
        },

        createArticle(){
            const form = new FormData();
            form.append('file', this.image);
            form.append('upload_preset', process.env.VUE_APP_CLOUDINARY_PRESET);
            form.append('api_key', process.env.VUE_APP_CLOUDINARY_API_KEY);

            Axios.post(process.env.VUE_APP_CLOUDINARY_URL, form)
            .then(response => {

                Axios.post(`${config.apiUrl}/articles`,{  
                        title: this.title,
                        content: this.content,
                        category_id: this.category,
                        imageUrl: response.data.secure_url
                    },
                    {
                        headers: {
                            Authorization: `Bearer ${this.$root.auth.token}`
                        }
                    }
                ).then(() => {
                    this.$noty.success('Article created successfuly');
                    this.$router.push("/");
                }).catch(error => {
                    this.$noty.error('Could not save creating article');
                    console.error(error);
                });
                
            }).catch(error => {
                console.error(error);
            });
        },

        getCategories(){
            const categories = localStorage.getItem('categories');
            if (categories) {
                this.categories = JSON.parse(categories);
                return;
            }

            Axios.get(`${config.apiUrl}/categories`)
                .then(response => {
                    this.categories = response.data.categories;
                    localStorage.setItem('categories', JSON.stringify(this.categories));
                })
        }
    }
}
</script>

