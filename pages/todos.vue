<template>
    <div>
        <div class="form">
            <form v-on:submit.prevent="add">
                <input v-model="name" type="text">
                <button>Add</button>
            </form>
        </div>
        <!-- データそのまま取り出す　{{ todos }} -->
        <ul>
            <li v-for="todo in todos" :key="todo.id">
                <!-- {{ todo }} -->
                <input type="checkbox"
                v-bind:checked="todo.done"
                @change="toggle(todo)">
                <span v-bind:class="{ done: todo.done }">
                    {{ todo.name}}, {{ todo.created }}
                </span>
                <button v-on:click="remove(todo.id)">Remove</button>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            name: '',
            done: false
        }
    },
    created: function() {
        this.$store.dispatch('todos/init')
    },
    methods: {
        add: function() {
            this.$store.dispatch('todos/add', this.name)
            this.name = '' // inputのデータをリセット
        },
        remove: function(id) {
            this.$store.dispatch('todos/remove', id)
        },
        toggle: function(todo) {
            this.$store.dispatch('todos/toggle', todo)
        }
    },
    computed: {
        todos: function() {
            return this.$store.state.todos.todos
        }
    }
}
</script>

<style>
li > span.done {
    text-decoration: line-through;
}
</style>