<template>
   <!-- props로 받은 데이터는 자식컴포넌트에서 직접 변경하지 말것, prop는 단방향임. -->
   <div 
        v-for="(todo, index) in todos"
        :key="todo.id"
        class="card mt-2"
      >
        <div class="card-body p-2 d-flex align-items-center">
          <div class="form-check flex-grow-1">
            <input 
              class="form-check-input"
              type="checkbox"
              :value="todo.completed"
              @change="toggleTodo(index)"
            >
            <label 
              class="form-check-label"
              :style="todo.completed ? todoStyle : {}"
              :class="{ todo: todo.completed}"
            >
              {{ todo.subject }}
            </label>
          </div>
          <div>
            <button 
              class="btn btn-danger btn-sm"
              @click="deleteTodo(index)"
            >
              Delete
            </button>
          </div>        
        </div>
      </div>
</template>
<script>
 export default {
    //  props: ['todos']
    props: {
        todos: {
            type: Array,
            required: true
        }
    },
    emits: ['toggle-todo', 'delete-todo'],
    setup(props, { emit }) {
        const toggleTodo = (index) => {
            emit('toggle-todo', index);
        }

        const deleteTodo = (index) => {
            emit('delete-todo', index);
        }
        return {
            toggleTodo,
            deleteTodo,
        }
    }
 }
</script>
<style>
</style>