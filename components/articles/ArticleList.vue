<template>
    <div>
        <div class="container">
      <h2 class="p-lg">Latest Articles</h2>
      <ul v-if="posts && posts.length">
        <li v-for="post in posts" :key="post.id">
          <NuxtLink :to="`/blog/${post.uid}`">
            {{ post.data.title }}
          </NuxtLink>
          <span v-if="post.data.publication_date">
            - {{ formatDate(post.data.publication_date) }}
          </span>
        </li>
      </ul>
      <p v-else>No articles found.</p>
    </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { usePrismic } from '@prismicio/vue'
  
  const { client } = usePrismic()
  const posts = ref([])
  
  const fetchPosts = async () => {
    try {
      const response = await client.getAllByType('blog_post', {
        orderings: [
          { field: 'document.first_publication_date', direction: 'desc' }
        ]
      })
      posts.value = response
    } catch (error) {
      console.error('Error fetching blog posts:', error)
    }
  }
  
  const formatDate = (date) => {
    return new Date(date).toLocaleDateString('en-US', {
      year: 'numeric',
      month: 'long',
      day: 'numeric'
    })
  }
  
  onMounted(fetchPosts)
  </script>
  
  <style scoped>
  /* Add your styles here */
  </style>