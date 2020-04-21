<template>
    <section class="real-app">
        <input
            type="text"
            class="add-input"
            autofocus="autofocus"
            placeholder="接下来要做什么？"
            v-model="input_content"
            @keyup.enter="addTodo"
        >
        <Item 
            :todo="todo"
            v-for="todo in filteredTodos"
            :key="todo.id"
            @del="delTodo"
        />
        <Tabs 
            :filter="filter"
            :todos="todos"
            @change="changeState"
            @clear="clearAllCompleted"
        />
    </section>
</template>
<script>
import Item from './item.vue'
import Tabs from './tabs.vue'
export default {
    components: {
        Item,
        Tabs,
    },
    data(){
        return {
            todos: [],
            id: 0,
            input_content: "",
            filter: "all"
        }
    },
    computed: {
        filteredTodos() {
            if (this.filter === 'all') {
                return this.todos
            }
            const completed = this.filter === 'completed'
            return this.todos.filter(todo => completed === todo.completed)
        }
    },
    methods: {
        addTodo() {
            if(this.input_content.trim() == "") {
                this.input_content = ""
                return 
            }
            this.todos.unshift({
                id: this.id++,
                content: this.input_content.trim(),
                completed: false
            })
            this.input_content = ""
        },
        delTodo(id) {
            this.todos.splice(this.todos.findIndex(todo => todo.id === id),1)
        },
        changeState(state) {
            this.filter = state
        },
        clearAllCompleted() {
            this.todos = this.todos.filter(todo => !todo.completed)
        }
    }
}
</script>

<style lang="stylus" scoped>
    .real-app{
        width 600px
        margin 0 auto
        box-shadow 0 0 5px #666
    }
    .add-input{
        position relative
        margin 0
        width 100%
        font-size 24px
        line-height 1.4em
        border 0
        outline none
        color inherit 
        padding 6px
        border 1px solid #999
        box-shadow inset 0 -1px 5px 0 rgba(0,0,0,0.4)
        box-sizing border-box
        padding 16px 16px 16px 60px
        border none 
        box-shadow inset 0 -2px 1px rgba(0,0,0,0.2)
    }
</style>