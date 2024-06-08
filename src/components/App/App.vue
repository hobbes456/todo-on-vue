<template>
    <main>
        <div>
            <h3>Задачник</h3>
        </div>
        <div>
            <div>
                <form @submit.prevent="addTodo">
                    <h3>Напишите свою задачу</h3>
                    <input 
                        type="text"
                        placeholder="Время объявить свои планы!"
                        v-model="text">
                    <input type="submit" value="Добавить задачу"/>
                </form>
            </div>
        </div>
        <div>
            <div>
                <h2 v-show="todos.length === 0">Нет задач</h2>
                <div>
                    <div 
                        v-for="todo in todos"
                        :class="`todo-item ${todo.done && 'done'}`"
                        :key="todo.id"
                    >
                        <label>
                            <input type="checkbox" v-model="todo.done" />
                        </label>
                        <div>
                            <input type="text" v-model="todo.value" />
                        </div>
                        <div>
                            <button @click="deleteTodo(todo)">Удалить</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup>
    import { ref, onMounted, watch } from 'vue';

    const todos = ref([]);
    const text = ref("");

    const addTodo = () => {
        if (text.value.trim() === "") return;

        todos.value.unshift({
            id: Date.now(),
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
            localStorage.setItem("todos", JSON.stringify(newTodo));
        },
        { deep: true }
    );

    onMounted(() => {
        todos.value = JSON.parse(localStorage.getItem("todos")) || [];
    });
</script>