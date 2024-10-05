<!-- PrismicRichTextContent.vue -->
<template>
    <div class="rich-text-content">
      <PrismicRichText :field="content" :serializer="serializer" />
    </div>
  </template>
  
  <script setup>
  import { PrismicRichText } from '@prismicio/vue'
  
  const props = defineProps({
    content: {
      type: Array,
      required: true
    }
  })
  
  const serializer = {
    heading1: ({ children }) => `<h1 class="h1">${children}</h1>`,
    heading2: ({ children }) => `<h2 class="h2">${children}</h2>`,
    heading3: ({ children }) => `<h3 class="h3">${children}</h3>`,
    heading4: ({ children }) => `<h4 class="h4">${children}</h4>`,
    heading5: ({ children }) => `<h5 class="h5">${children}</h5>`,
    heading6: ({ children }) => `<h6 class="h6">${children}</h6>`,
    paragraph: ({ children }) => `<p class="p-rg">${children}</p>`,
    strong: ({ children }) => `<strong class="font-bold">${children}</strong>`,
    em: ({ children }) => `<em class="italic">${children}</em>`,
    listItem: ({ children }) => `<li class="p-rg">${children}</li>`,
    oListItem: ({ children }) => `<li class="p-rg">${children}</li>`,
    list: ({ children }) => `<ul class="list-disc pl-5 mb-4">${children}</ul>`,
    oList: ({ children }) => `<ol class="list-decimal pl-5 mb-4">${children}</ol>`,
  }
  </script>
  
  <style scoped>
  .rich-text-content {
    /* Add any additional styles for the content wrapper */
  }
  
  /* You can add more specific styles here if needed */
  .rich-text-content :deep(h1) {
    margin-bottom: 1rem;
  }
  
  .rich-text-content :deep(h2),
  .rich-text-content :deep(h3),
  .rich-text-content :deep(h4),
  .rich-text-content :deep(h5),
  .rich-text-content :deep(h6) {
    margin-top: 2rem;
    margin-bottom: 1rem;
  }
  
  .rich-text-content :deep(p) {
    margin-bottom: 1rem;
  }
  
  .rich-text-content :deep(ul),
  .rich-text-content :deep(ol) {
    margin-bottom: 1rem;
  }
  
  .rich-text-content :deep(li) {
    margin-bottom: 0.5rem;
  }
  </style>