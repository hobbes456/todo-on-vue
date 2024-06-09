<script setup>
    import { ref, onMounted, watch } from 'vue';
    import EditInput from '../EditInput/EditInput.vue';
    import "./App.scss";

    const todos = ref([]);
    const text = ref("");

    const addTodo = () => {
        if (text.value.trim() === "") return;

        todos.value.unshift({
            id: Date.now(),
            value: text.value,
            isDone: false,
            isEdit: false,
        })

        text.value = "";
    };

    const deleteTodo = (todo) => {
        todos.value = todos.value.filter((element) => element !== todo);
    };

    const showEditTodo = (todo) => {
        todo.isEdit = true;
    }

    watch(
        todos,
        (newTodo) => {
            localStorage.setItem("todos", JSON.stringify(newTodo));
        },
        { deep: true }
    );

    onMounted(() => {
        todos.value = JSON.parse(localStorage.getItem("todos")) || [];
    });
</script>

<template>
    <main class="app">
        <h3 class="app__title">Задачник</h3>
        <form class="app__form" @submit.prevent="addTodo">
            <h1 class="app__subtitle">Напишите свою задачу</h1>
            <div class="app__enter">
                <input 
                    class="app__input"
                    type="text"
                    placeholder="Время объявить свои планы!"
                    v-model="text">
                <input 
                    class="app__btn" 
                    type="submit" 
                    value="Добавить задачу"/>
            </div>
        </form>
        <h1 v-show="todos.length === 0" class="app__subtitle" >Нет задач</h1>
        <div class="app__todos">
            <div 
                v-for="todo in todos"
                class="app__todo"
                :class="{'app__todo_done': todo.isDone}"
                :key="todo.id">
                <input type="checkbox" v-model="todo.isDone" />
                <div>
                    <EditInput 
                        v-if="todo.isEdit" 
                        class="app__input" 
                        :todo="todo"
                        v-model="todo.value"/>
                    <div 
                        v-else 
                        class="app__task" 
                        @dblclick="showEditTodo(todo)"
                    >{{todo.value}}</div>
                </div>
                <button class="app__btn" @click="deleteTodo(todo)">Х</button>
            </div>
        </div>
    </main>
</template>