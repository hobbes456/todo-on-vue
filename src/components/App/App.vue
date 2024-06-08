<template>VUE APP</template>

<script setup>
    import { ref, onMounted, watch } from 'vue';

    const todos = ref([]);
    const text = ref("");

    const addTodo = () => {
        if (text.value.trim() === "") return;

        todos.value.unshift({
            value: text.value,
            done: false,
        })

        text.value = "";
    }
</script>