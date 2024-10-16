<template>
    <div>
        <div class="container">
            <div class="pt-10 relative">
                <h2 class="text-4xl">Latest Articles</h2>
                <span class="h-[1px] w-full bg-neutral-500 absolute top-0 left-0"></span>
            </div>
            <div class="pt-8">
                <ul v-if="posts && posts.length">
                    <div class="flex flex-row gap-4 border rounded-2xl border-neutral-500 border-solid p-8">
                        <li class="flex flex-col gap-6" v-for="post in posts" :key="post.id">
                            <div class="grid grid-cols-5 gap-10">
                                <div class="col-span-2" v-if="post.data.slices && post.data.slices.length > 0">
                                    <div class="w-full h-full" v-for="slice in post.data.slices" :key="slice.id">
                                        <div v-if="slice.slice_type === 'featured_image'" class="w-full h-full overflow-hidden rounded-2xl">
                                            <NuxtImg class="w-full h-full object-cover" :src="slice.primary.featured_image.url" :alt="slice.primary.featured_image.alt || ''" />
                                        </div>
                                    </div>
                                </div>
                                <div class="col-span-3 flex flex-col gap-4">
                                    <span v-if="post.data.publication_date"> {{ formatDate(post.data.publication_date) }} </span>

                                    <div class="blog_title_wrap">
                                        <p style="font-weight: 600" class="text-3xl">{{ post.data.title }}</p>
                                    </div>
                                    <div class="blog_summary_wrap">
                                        <p class="p-sm">{{ post.data.summary }}</p>
                                    </div>
                                    <div class="flex flex-row">
                                        <PrimaryButton :to="`/blog/${post.uid}`" text="Read More" />
                                    </div>
                                </div>
                            </div>
                        </li>
                    </div>
                </ul>
                <p v-else>No articles found.</p>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { usePrismic } from "@prismicio/vue";
import { Button } from "@/components/ui/button";
import PrimaryButton from "../globals/PrimaryButton.vue";

const { client } = usePrismic();
const posts = ref([]);

const fetchPosts = async () => {
    try {
        const response = await client.getAllByType("blog_post", {
            orderings: [{ field: "document.first_publication_date", direction: "desc" }],
        });
        posts.value = response;
    } catch (error) {
        console.error("Error fetching blog posts:", error);
    }
};

const formatDate = (date) => {
    return new Date(date).toLocaleDateString("en-US", {
        year: "numeric",
        month: "long",
        day: "numeric",
    });
};

onMounted(fetchPosts);
</script>
