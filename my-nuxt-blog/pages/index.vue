<template>
  <div>
    <h1>Blog</h1>
    <select v-model="selectedCategory">
      <option value="">All categories</option>
      <option v-for="cat in categories" :key="cat">{{ cat }}</option>
    </select>

    <div v-if="pending">Loading posts...</div>
    <div v-else-if="error">Error: {{ error.message }}</div>
    <div v-else>
      <div v-for="post in filteredPosts" :key="post.id" style="border:1px solid #ccc; margin:10px; padding:10px;">
        <h2>{{ post.title }}</h2>
        <p><strong>{{ post.author }}</strong></p>
        <p>{{ post.snippet || post.content.substring(0,100) }}...</p>
        <NuxtLink :to="`/blog/${post.id}`">Read more</NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const { data: posts, pending, error } = await useFetch('http://localhost:1337/api/blog-posts?populate=*')
const selectedCategory = ref('')

const categories = computed(() => {
  if (!posts.value?.data) return []
  return [...new Set(posts.value.data.map(p => p.category))]
})

const filteredPosts = computed(() => {
  if (!posts.value?.data) return []
  if (!selectedCategory.value) return posts.value.data
  return posts.value.data.filter(p => p.category === selectedCategory.value)
})
</script>