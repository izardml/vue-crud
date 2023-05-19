<script setup>

import { ref } from 'vue'
import { useRouter } from 'vue-router';
import api from '../../api'

const router = useRouter()

const image = ref('')
const title = ref('')
const content = ref('')
const errors = ref([])

const handleFileChange = (e) => {
    image.value = e.target.files[0]
}

const storePost = async() => {
    let formData = new FormData()

    formData.append('image', image.value)
    formData.append('title', title.value)
    formData.append('content', content.value)

    await api.post('/api/posts', formData).then(() => {
        router.push({ path: '/posts' })
    }).catch((error) => {
        errors.value = error.response.data
        console.log(errors.value)
    })
}

</script>

<template>
    <div class="container my-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <form @submit.prevent="storePost()">
                            <div class="mb-3">
                                <label for="image" class="form-label fw-bold">Image</label>
                                <input type="file" name="image" id="image" class="form-control" @change="handleFileChange($event)">
                                <div v-if="errors.image" class="alert alert-danger mt-2">
                                    <span>{{ errors.image[0] }}</span>
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="title" class="form-label fw-bold">Title</label>
                                <input type="text" name="title" id="title" class="form-control" v-model="title" placeholder="Title Post">
                                <div v-if="errors.title" class="alert alert-danger mt-2">
                                    <span>{{ errors.title[0] }}</span>
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="content" class="form-label fw-bold">Content</label>
                                <textarea name="content" id="content" cols="30" rows="5" class="form-control" v-model="content" placeholder="Content Post"></textarea>
                                <div v-if="errors.content" class="alert alert-danger mt-2">
                                    <span>{{ errors.content[0] }}</span>
                                </div>
                            </div>
                            <button type="submit" class="btn btn-md btn-primary rounded-sm shadow border-0">Save</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>