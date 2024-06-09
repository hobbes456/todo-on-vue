<script setup>
    import { ref, onMounted } from 'vue';

    defineProps({
        todo: Object,
    });

    const input = ref(null);
    const model = defineModel();

    const closeEditTodo = (todo) => {
        if (todo.value.trim() === "") return;

        todo.isEdit = false;
    };

    onMounted(() => input.value.focus());
</script>

<template>
    <input 
        type="text" 
        ref="input"
        @change="closeEditTodo(todo)"
        @blur="closeEditTodo(todo)"
        v-model="model"/>
</template>