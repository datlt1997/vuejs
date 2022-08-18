<template>
    <AddTodo @add-todo = "addItem"/>
    <TodoItem 
        v-for="todo in todos" 
        :key="todo.id" 
        :todoProps ="todo" 
        @item-completed = "markCompleted"
        @item-delete = "deleteItem"
    />
</template>

<script>
import { ref} from 'vue'
import { v4 as uuidv4} from 'uuid'
import TodoItem from './TodoItem.vue'
import axios from 'axios'
import AddTodo from './AddTodo.vue'
export default {
    name: "Todos",
    components: {TodoItem, AddTodo},
    setup() {
        const todos = ref([])

        const getAllTodos = async () => {
            try {
                const res = await axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
                console.log(res);
                todos.value = res.data
            } catch (error) {
                console.log(error)
            }
        }
        getAllTodos()

        const markCompleted = id => {
            todos.value = todos.value.map(todo => {
                if (todo.id === id) todo.completed = !todo.completed
                return todo
            })
        }

        const deleteItem = async (id) => {
            try {
                await axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
                todos.value = todos.value.filter(todo => todo.id !== id)
            } catch (error) {
                console.log(error)
            }
        }

        const addItem = async (newTodo) => {
            try {
                const res = await axios.post('https://jsonplaceholder.typicode.com/todos', newTodo)
                todos.value.push(res.data)
            } catch (error) {
                console.log(error)
            }
        }
        return {
            todos,
            markCompleted,
            deleteItem,
            addItem
        }
    }
}
</script>

<style></style>