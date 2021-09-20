<template>
    <div>
        <ul>
            <TodoItem
                    v-for="(todo, i) in todos"
                    v-bind:todo="todo"
                    v-bind:key="todo.id"
                    v-bind:index="i"
                    v-on:remove-todo="removeTodo"
                    v-on:patch-todo="patchTodo"
            />
        </ul>
        <div v-if="todos.length">
            Total: {{todos.length}}
        </div>
    </div>
</template>

<script>
    import TodoItem from "@/components/TodoItem"

    export default {
        props: ['todos'],
        components: {
            TodoItem
        },
        methods: {
            removeTodo(id) {
                this.$emit('remove-todo', id)
            },

            patchTodo(todo) {
                this.$emit('patch-todo', todo)
            },
        },

        filters: {

        }
    }
</script>

<style scoped>
    ul {
        list-style: none;
        margin: 0;
        padding: 0;
    }
</style>