<template>
  <div>
    <h1>Search</h1>
    <input v-model="query" placeholder="Search by title or author..." />
    <div v-if="pending">Loading...</div>
    <div v-else-if="error">Error: {{ error.message }}</div>
    <div v-else>
      <div v-for="post in results" :key="post.id" style="border:1px solid #ccc; margin:10px; padding:10px;">
        <h2>{{ post.title }}</h2>
        <p>{{ post.author }}</p>
        <NuxtLink :to="`/blog/${post.id}`">Read</NuxtLink>
      </div>
      <div v-if="results.length === 0 && query">No results found.</div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const { data: posts, pending, error } = await useFetch('http://localhost:1337/api/blog-posts?populate=*')
const query = ref('')

const results = computed(() => {
  if (!posts.value?.data) return []
  if (!query.value) return []
  const q = query.value.toLowerCase()
  return posts.value.data.filter(p => 
    p.title.toLowerCase().includes(q) || p.author.toLowerCase().includes(q)
  )
})
</script>