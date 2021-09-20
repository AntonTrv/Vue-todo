<template>
    <div id="todo">
        <router-link to="/">Home</router-link>
        <AddTodo
                @add-todo="addTodo"
        />
        <select v-model="filter">
            <option value="all">All</option>
            <option value="completed">Completed</option>
            <option value="not completed">Not completed</option>
        </select>
        <hr>
        <Loader v-if="loading"/>
        <TodoList
                v-else
                v-bind:todos="filteredTodos"
                @patch-todo="patchTodo"
                @remove-todo="removeTodo"
        />
        <div v-if="!filteredTodos.length">
            <p>No todos found</p>
        </div>
    </div>
</template>

<script>
    import TodoList from '@/components/TodoList'
    import AddTodo from '@/components/AddTodo'
    import Loader from '@/components/Loader'
    import axios from "axios";

    export default {
        name: 'App',
        data() {
            return {
                todos: [],
                loading: true,
                filter: 'all'
            }
        },
        components: {
            TodoList,
            AddTodo,
            Loader
        },

        async mounted() {
            try {
                const results = await axios.get('http://localhost:3000/todos-items');
                this.todos = results.data;
                this.isLoading();
            } catch (e) {
                console.error(e)
            }
        },

        methods: {
            async addTodo(newTodo) {
                try {
                    this.isLoading();
                    const results = await axios.post('http://localhost:3000/todos-items', newTodo);
                    this.todos = [...this.todos, results.data];
                    this.isLoading();
                } catch (e) {
                    console.error(e)
                }
            },

            async removeTodo(id) {
                try {
                    this.isLoading();
                    const results = await axios.delete(`http://localhost:3000/todos-items/${id}`);
                    this.todos = this.todos.filter(t => t.id !== id);
                    this.isLoading();
                } catch (e) {
                    console.error(e)
                }
            },

            async patchTodo(todo) {
                this.isLoading();
                const result = axios.put(`http://localhost:3000/todos-items/${todo.id}`, {"id":todo.id,"title": todo.title, "completed": todo.completed});
                this.isLoading();
            },

            isLoading() {
                this.loading = !this.loading;
            }
        },
        computed: {
            filteredTodos() {
                let filter
                if (this.filter === 'all') {
                    filter = this.todos
                }

                if (this.filter === 'completed') {
                    filter = this.todos.filter(t => t.completed)
                }

                if (this.filter === 'not completed') {
                    filter = this.todos.filter(t => !t.completed)
                }
                return filter
            }
        }
    }
</script>

<style>
    select {
        margin: 20px 0;
    }
</style>