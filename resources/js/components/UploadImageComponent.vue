<template>
    <div class="mt-2">
         <div class="card-group">
            <div class="card" v-for="item in galleryList" :key="item.id">
                <img :src="item.image" class="card-img-top" alt="..." height="300px" width="300px">
                <div class="card-body">
                    <h5 class="card-title">{{ item.name }}</h5>
                    <p class="card-text">{{ item.description}}</p>
                    <p class="card-text"><small class="text-muted">{{ item.updated_at}}</small></p>
                </div>
            </div>
        </div>
        <hr>
        <h1>Form</h1>
        <div class="form-group">
            <label for="name">Name</label>
            <input type="text" class="form-control" placeholder="Input Name" v-model="name">
        </div>
         <div class="form-group">
            <label for="name">Description</label>
            <input type="text" class="form-control" placeholder="Input Description" v-model="description">
        </div>
         <div class="form-group">
            <label for="name">Image</label>
            <input type="file" class="form-control-file" @change="onFileChange">
        </div>
        <div class="form-group">
            <button class="btn btn-primary pull-right" @click="submit">SUBMIT</button>
        </div>
    </div>
</template>

<script>
export default {
    props: ['gallery'],
    data() {
        return {
            galleryList: this.gallery,
            name: null,
            description: null,
            image: null
        }
    },
    methods: {
        onFileChange() {
            this.image = event.target.files[0];
        },
        submit() {
            const vm = this;
            const config = {
                header: { "content_type": "multipart/form-data" }
            }

            let formData = new FormData();
            formData.append('name', vm.name);
            formData.append('description', vm.description);
            formData.append('image', vm.image);

            axios.post('gallery',formData, config)
            .then(function(response){
                vm.galleryList.push(response.data.data)
            })
            .catch(function(error){
                console.log(error);
            });
        }
    }
}
</script>
