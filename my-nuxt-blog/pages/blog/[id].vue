<template>
  <div v-if="pending">Loading posts...</div>
  <div v-else-if="error">Error: {{ error.message }}</div>
  <div v-else-if="post">
    <h1>{{ post.title }}</h1>
    <p><strong>By {{ post.author }}</strong></p>
    <div style="white-space: pre-wrap;">{{ post.content }}</div>
    <NuxtLink to="/">← Back to home</NuxtLink>
  </div>
  <div v-else>Post not found.</div>
</template>

<script setup>
import { computed } from 'vue'

const route = useRoute()
const numericId = parseInt(route.params.id)  // e.g., 2 from /blog/2

const { data: allPosts, pending, error } = await useFetch('http://localhost:1337/api/blog-posts?populate=*')

const post = computed(() => {
  if (!allPosts.value?.data) return null
  return allPosts.value.data.find(p => p.id === numericId)
})
</script>