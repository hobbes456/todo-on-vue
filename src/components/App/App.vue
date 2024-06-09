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
                        :key="todo.id">
                        <label>
                            <input type="checkbox" v-model="todo.done" />
                        </label>
                        <div>
                            <EditInput v-if="todo.isEdit" v-model="todo.value" :correct-todo="todo"/>
                            <div v-else @dblclick="showEditTodo(todo)">{{todo.value}}</div>
                        </div>
                        <div>
                            <button @click="deleteTodo(todo)">Х</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup>
    import { ref, onMounted, watch } from 'vue';

    import EditInput from '../EditInput/EditInput.vue';

    const todos = ref([]);
    const text = ref("");

    const addTodo = () => {
        if (text.value.trim() === "") return;

        todos.value.unshift({
            id: Date.now(),
            value: text.value,
            done: false,
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