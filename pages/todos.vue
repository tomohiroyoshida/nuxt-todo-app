<template>
    <div>
        <div class="form">
            <form v-on:submit.prevent="add">
                <input v-model="name" type="text" placeholder="Add Todo Here!">
                <button>Add</button>
            </form>
        </div>
        <!-- データそのまま取り出す　{{ todos }} -->
        <ul>
            <li v-for="todo in todos" :key="todo.id">
                <span v-if="todo.created">
                    <!-- {{ todo }} -->
                    <input type="checkbox"
                    v-bind:checked="todo.done"
                    @change="toggle(todo)">
                    <span v-bind:class="{ done: todo.done }">
                        {{ todo.name }}, {{ todo.created.toDate() | dateFilter }}
                    </span>
                    <button v-on:click="remove(todo.id)">Remove</button>
                </span>
            </li>
        </ul>
    </div>
</template>

<script>
import moment from 'moment'
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
            // return this.$store.state.todos.todos
            return this.$store.getters['todos/orderedTodos']
        }
    },
    filters: {
        dateFilter: function(date) {
            return moment(date).format('YYYY/MM/DD HH:mm:ss')
        }
    }
}
</script>

<style>
li > span > span.done {
    text-decoration: line-through;
}
.form {
    margin: 15px;
}
li {
    margin-bottom: 3px;
}
</style>