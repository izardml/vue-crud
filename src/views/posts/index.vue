<script setup>

import { ref, onMounted } from 'vue'
import api from '../../api'

const posts = ref([])
const fetchDataPosts = async () => {
    await api.get('/api/posts').then(response => {
        posts.value = response.data.data.data
        console.log(response.data.data.data)
    })
}

onMounted(() => {
    fetchDataPosts()
})

const deletePost = async(id) => {
    await api.delete(`/api/posts/${id}`).then(() => {
        fetchDataPosts()
    })
}

</script>

<template>
    <div class="container my-5">
        <div class="row">
            <div class="col-md-12">
                <router-link :to="{ name: 'posts.create' }" class="btn btn-md btn-success rounded shadow border-0 mb-3">Add New Post</router-link>
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <table class="table table-bordered">
                            <thead class="bg-dark text-white">
                                <tr>
                                    <th>Image</th>
                                    <th>Title</th>
                                    <th>Content</th>
                                    <th style="width: 15%">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-if="posts.length == 0">
                                    <td colspan="4" class="text-center">
                                        <div class="alert alert-danger mb-0">
                                            Data Belum Tersedia!
                                        </div>
                                    </td>
                                </tr>
                                <tr v-else v-for="(post, index) in posts" :key="index">
                                    <td class="text-center">
                                        <img :src="post.image" :alt="post.title" width="200" class="rounded-3">
                                    </td>
                                    <td>{{ post.title }}</td>
                                    <td>{{ post.content }}</td>
                                    <td class="text-center">
                                        <router-link :to="{ name: 'posts.edit', params: { id: post.id } }" class="btn btn-sm btn-primary rounded-sm shadow border-0 me-2">Edit</router-link>
                                        <button @click.prevent="deletePost(post.id)" class="btn btn-sm btn-danger rounded-sm shadow border-0">Delete</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>