<template>
    <div id="todo" >
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
        <popUp v-bind:error="error" v-on:remove-popup="notifyAboutError"/>
    </div>
</template>

<script>
    import TodoList from '@/components/TodoList'
    import AddTodo from '@/components/AddTodo'
    import Loader from '@/components/Loader'
    import popUp from "../components/popUp";
    import {apiGet, apiAdd, apiDelete, apiPatch} from "../../api/methods";

    export default {
        name: 'App',
        data() {
            return {
                todos: [],
                loading: true,
                filter: 'all',
                error: null
            }
        },
        components: {
            TodoList,
            AddTodo,
            Loader,
            popUp
        },

        async mounted() {
            const [error, results] = await apiGet();
            if (error) {
                this.notifyAboutError(error);
            } else {
                this.todos = results;
            }
            this.isLoading();
        },

        methods: {
            async addTodo(newTodo) {
                this.isLoading();
                const [error, results] = await apiAdd(newTodo);
                if(error) {
                    this.notifyAboutError(error)
                } else  {
                    this.todos = [...this.todos, results];
                }
                this.isLoading();
            },

            async removeTodo(id) {
                this.isLoading();
                const [error] = await apiDelete(id);
                if(error) {
                    this.notifyAboutError(error)
                } else {
                    this.todos = this.todos.filter(t => t.id !== id);
                }
                this.isLoading();
            },

            async patchTodo(todo) {
                this.isLoading();
                const [error] = await apiPatch(todo);
                if (error) {
                    this.notifyAboutError(error)
                }
                this.isLoading();
            },

            isLoading() {
                this.loading = !this.loading;
            },

            notifyAboutError(error) {
                this.error = error || null;
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