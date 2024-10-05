<template>
    <Nav />
    <div v-if="article" class="article-container">
      <h1 class="h3">{{ article.data.title }}</h1>
      <div class="article-meta">
        <span v-if="article.data.publication_date">
          Published on: {{ formatDate(article.data.publication_date) }}
        </span>
      </div>
      
      <div v-if="article.data.featured" class="featured-tag">Featured</div>
  
      <div class="article-content">
        <template v-for="(slice, index) in article.data.slices" :key="index">
          <div v-if="slice.slice_type === 'embedded_video'" class="embedded-video">
            <!-- Implement video embedding logic here -->
            <div v-html="slice.primary.embedded_video"></div>
          </div>
  
          <div v-else-if="slice.slice_type === 'featured_image'" class="featured-image">
            <img :src="slice.primary.featured_image.url" :alt="slice.primary.featured_image.alt || ''" />
          </div>
  
          <blockquote v-else-if="slice.slice_type === 'quote'" class="quote">
            {{ slice.primary.quote }}
          </blockquote>
  
          <div v-else-if="slice.slice_type === 'rich_text_block'" class="rich-text-block">
            <PrismicRichTextContent :content="slice.primary.content" />
          </div>
        </template>
      </div>
      
      <div class="article-navigation">
        <NuxtLink to="/" class="back-link">← Back to Articles</NuxtLink>
      </div>
    </div>
    <div v-else-if="error">Error: {{ error }}</div>
    <div v-else>Loading article...</div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { usePrismic } from '@prismicio/vue'
  import PrismicRichTextContent from '~/components/articles/PrismicRichTextContent.vue'
  
  const route = useRoute()
  const { client } = usePrismic()
  const article = ref(null)
  const error = ref(null)
  
  const fetchArticle = async () => {
    try {
      const response = await client.getByUID('blog_post', route.params.uid)
      article.value = response
      console.log('Fetched article:', JSON.stringify(response, null, 2))
      console.log('Slices:', response.data.slices)
    } catch (err) {
      console.error('Error fetching article:', err)
      error.value = 'Failed to load article'
    }
  }
  
  const formatDate = (date) => {
    return new Date(date).toLocaleDateString('en-US', {
      year: 'numeric',
      month: 'long',
      day: 'numeric'
    })
  }
  
  onMounted(fetchArticle)
  </script>
  
  <style scoped>
  .article-container {
    max-width: 880px;
    margin: 0 auto;
    padding: 20px;
  }
  
  .article-meta {
    font-style: italic;
    color: #666;
    margin-bottom: 20px;
  }
  
  .featured-tag {
    background-color: #ffd700;
    color: #000;
    padding: 5px 10px;
    display: inline-block;
    margin-bottom: 20px;
  }
  
  .article-content {
    line-height: 1.6;
  }
  
  .featured-image img {
    max-width: 100%;
    height: auto;
    margin-bottom: 1rem;
  }
  
  .quote {
    font-style: italic;
    border-left: 4px solid #ccc;
    padding-left: 20px;
    margin: 20px 0;
  }
  
  .embedded-video {
    margin-bottom: 1rem;
  }
  
  .rich-text-block {
    margin-bottom: 2rem;
  }
  
  .article-navigation {
    margin-top: 30px;
  }
  
  .back-link {
    text-decoration: none;
    color: #007bff;
  }
  </style>