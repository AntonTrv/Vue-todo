<template>
    <div id="app">
        <h1>Todo App</h1>
        <hr>
       <router-view />
    </div>
</template>

<script>
    export default {
        name: 'App',
        data() {
            return {
                 todos: [],
            }
        },
        components: {
        },

        methods: {
            addTodo(newTodo) {
                this.todos.push(newTodo)
            },
            removeTodo(id) {
                this.todos = this.todos.filter(t => t.id !== id)
            },
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
