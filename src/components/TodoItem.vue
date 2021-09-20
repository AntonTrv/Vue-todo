<template>
    <li @click="todo.completed = !todo.completed, $emit('patch-todo',todo)">
        <span v-bind:class="{done: todo.completed}">
            <strong>{{index + 1}}</strong>
            {{todo.title | capitalizer}}
        </span>
        <button class="rm" @click="$emit('remove-todo', todo.id)">&times;</button>
    </li>
</template>

<script>

    export default {
        props: {
            todo: {
                type: Object,
                require: true
            },
            index: Number
        },

        filters: {
            capitalizer([first, ...rest], locale = navigator.language) {
                return first.toLocaleUpperCase(locale) + rest.join('')
            }
        }
    }
</script>

<style scoped>
    li {
        border: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        padding: .5rem 2rem;
        margin-bottom: 1rem;
        cursor: pointer;
    }

    .rm {
       background: red;
        color: #fff;
        border-radius: 50%;
        font-weight: bold;
    }

    input {
        margin-right: 1rem;
    }

    .done {
        text-decoration: line-through;
    }
</style>