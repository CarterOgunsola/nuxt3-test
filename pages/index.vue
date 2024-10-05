<script lang="ts" setup>
import { useGlobalStore } from "~/stores";

import ArticleList from '~/components/articles/ArticleList.vue';

const localCount = ref(0);

const store = useGlobalStore();

const click = () => {
    store.increment();
    localCount.value++;
};
</script>

<template>
    <Nav />
    <LandingHome />
    <ArticleList />
</template>

<style></style>
