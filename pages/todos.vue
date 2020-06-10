<template>
    <div>
        <div class="form">
            <form v-on:submit.prevent="add">
                <input v-model="name" type="text" placeholder="Add Todo Here!">
                <button>Add</button>
            </form>
        </div>
        <!-- {{ todos }} -->
        <ol>
            <li v-for="todo in todos" :key="todo.id">
                <span v-if="todo.created">
                    <input type="checkbox"  v-bind:checked="todo.done"  @change="toggle(todo)">
                    <span v-bind:class="{ done: todo.done }">
                        [Title] {{ todo.name }} [Date] {{ todo.created.toDate() | dateFilter }}
                    </span>
                    <button v-on:click="remove(todo.id)">Remove</button>
                </span>
            </li>
        </ol>
    </div>
</template>

<script>
import moment from 'moment'
import firebase from 'firebase'
import db from '../plugins/firebase_config'

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
            // 時系列順にソート
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
    color: darkblue;
}
li > span {
    color: crimson;
}
.form {
    margin: 15px;
}
li {
    margin-bottom: 5px;
}
</style>
