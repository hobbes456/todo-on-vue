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
    };

    const deleteTodo = (todo) => {
        todos.value = todos.value.filter((element) => element !== todo);
    };

    watch(
        todos,
        (newTodo) => {
            localStorage.setItem("todos", JSON.stringify(newTodo))
        },
        { deep: true }
    );

    onMounted(() => {
        todos.value = JSON.parse(localStorage.getItem("todos")) || [];
    });
</script>