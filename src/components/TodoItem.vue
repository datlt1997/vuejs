<template>
  <p :class="['todo-items', todoProps.completed ? 'is-completed' : '']">
    <input 
    type="checkbox" 
    :checked="todoProps.completed" 
    @change="markItemCompleted"
    />
    {{ todoProps.title}}
    <button class="delete-btn" @click="deleteItem">Delete</button>
  </p>
</template>

<script>

import { ref } from 'vue'

export default {
    name: "TodoItem",
    props: ['todoProps'],
    setup(props, context) {
        const markItemCompleted = () => {
            context.emit('item-completed', props.todoProps.id)
        }

        const deleteItem = () => {
            context.emit('item-delete', props.todoProps.id)
        }
        return {
            markItemCompleted,
            deleteItem
        }
    }
}
</script>

<style>
.todo-items {
    background: #f6f6f6;
    padding: 10px;
    margin: 0;
    border-bottom: 1px dotted #ccc
}
.delete-btn {
    background: red;
    color: white;
    border: none;
    cursor: pointer;
    float: right;
}

.is-completed {
    text-decoration: line-through;
}

</style>