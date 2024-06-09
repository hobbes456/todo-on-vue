<template>
    <input 
        type="text" 
        v-model="model"
        ref="input"
        @change="closeEditTodo(correctTodo)"
        @blur="closeEditTodo(correctTodo)"/>
</template>

<script setup>
    import { ref, onMounted } from 'vue';

    defineProps({
        correctTodo: Object,
    })

    const input = ref(null);
    const model = defineModel();

    const closeEditTodo = (todo) => {
        todo.isEdit = false;
    }

    onMounted(() => input.value.focus());
</script>