<template>
    <h1 class="text-center">
        {{ $route.params.id }}
    </h1>
</template>

<script>
import config from "@/config";
import Axios from 'axios';

export default {
    mounted() {
        this.getArticle();
    }, methods : {
        getArticle(){
            console.log(`${this.$route.params.id}`);
            Axios.get(`${config.apiUrl}/article/${this.$route.params.id}`).then(response => {
                console.log(response)
            })
        }
    }
}
</script>
