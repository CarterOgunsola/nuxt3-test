<template>
    <NuxtLink
        :to="to"
        class="px-4 py-2 bg-neutral-800 text-white rounded-full transition-all duration-300 ease-in-out hover:bg-neutral-700 hover:text-neutral-200 hover:shadow-lg hover:scale-101 hover:ring-1 hover:ring-neutral-600 focus:outline-none focus:ring-1 focus:ring-neutral-500"
        :class="customClass"
    >
        <slot>{{ text }}</slot>
    </NuxtLink>
</template>

<script setup>
defineProps({
    to: {
        type: String,
        required: true,
    },
    text: {
        type: String,
        default: "Button Text",
    },
    customClass: {
        type: String,
        default: "",
    },
});
</script>
