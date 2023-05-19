<script setup>

import { ref, onMounted } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import api from '../../api'

const router = useRouter()
const route = useRoute()

const image = ref('')
const title = ref('')
const content = ref('')
const errors = ref([])

onMounted(async() => {
    await api.get(`/api/posts/${route.params.id}`).then(res => {
        title.value = res.data.data.title
        content.value = res.data.data.content
    })
})

const handleFileChange = (e) => {
    image.value = e.target.files[0]
}

const updatePost = async() => {
    let formData = new FormData()

    formData.append('image', image.value)
    formData.append('title', title.value)
    formData.append('content', content.value)
    formData.append('_method', 'PATCH')

    await api.post(`/api/posts/${route.params.id}`, formData).then(() => {
        router.push({ path: '/posts' })
    }).catch((error) => {
        errors.value = error.response.data
    })
}

</script>

<template>
    <div class="container my-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <form @submit.prevent="updatePost()">
                            <div class="mb-3">
                                <label for="image" class="form-label">Image</label>
                                <input type="file" name="image" id="image" class="form-control" @change="handleFileChange($event)">
                                <div v-if="errors.image" class="alert alert-danger mt-2">
                                    <span>{{ errors.image[0] }}</span>
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="title" class="form-label">Title</label>
                                <input type="text" name="title" id="title" class="form-control" v-model="title" placeholder="Title Post">
                                <div v-if="errors.title" class="alert alert-danger mt-2">
                                    <span>{{ errors.title[0] }}</span>
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="content" class="form-label">Content</label>
                                <textarea name="content" id="content" cols="30" rows="5" class="form-control" v-model="content" placeholder="Content Post"></textarea>
                                <div v-if="errors.content" class="alert alert-danger mt-2">
                                    <span>{{ errors.content[0] }}</span>
                                </div>
                            </div>
                            <button type="submit" class="btn btn-md btn-primary rounded-sm shadow border-0">Update</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>