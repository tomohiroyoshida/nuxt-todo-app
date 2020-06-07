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
                {{ todo.done  }}, {{ todo.name}}, {{ todo.created }}
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
            this.name = ''
        }
    },
    computed: {
        todos: function() {
            return this.$store.state.todos.todos
        }
    }
}
</script>