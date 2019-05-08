<template>
    <div class="container">
        <div class="row">
            <div class="col-md-8 offset-md-2">
                <div class="card my-5">
                    <div class="card-body">
                        <input type="text" placeholder="Title" class="form-control mb-3">
                        <wysiwyg v-model="content"/>
                        <div class="text-center">
                            <button class="btn btn-success mt-3">Create article</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            content: ""
        }
    }
    
}
</script>

