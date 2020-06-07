<template>
    <div>
        <div class="form">
            <form v-on:submit.prevent="add">
                <input v-model="name" type="text">
                <button>Add</button>
            </form>
        </div>
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
    }
}
</script>