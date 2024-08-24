<template>
    <div>
        <h1>Временная метка (SSR + Клиент):</h1>
        <p>Временная метка с сервера: {{ serverTimestamp }}</p>
        <p>Текущая временная метка: {{ clientTimestamp }}</p>

        <h2>Список постов</h2>
        <ul>
            <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
        </ul>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useAsyncData } from '#app';

const serverTimestamp = ref('');
const clientTimestamp = ref('');

// Getting initial timestamp
const timestamp = new Date().toISOString();
serverTimestamp.value = timestamp;

// Update timestamp on client func
const updateClientTimestamp = () => {
    clientTimestamp.value = new Date().toISOString();
};

onMounted(() => {
    updateClientTimestamp();
    setInterval(updateClientTimestamp, 10000); // Update every 10 seconds
});

// Getting content through API JSONPlaceholder
const { data: posts } = await useAsyncData('posts', () =>
    $fetch('https://jsonplaceholder.typicode.com/posts'),
);
</script>
