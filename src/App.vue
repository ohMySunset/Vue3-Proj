<template>
  <div>
    <!-- 
      v-show : 모든 엘리먼트를 렌더링 한 후 판별 (초기 렌더 비용이 많이 듬)
               자주 렌더링이 되어야 하는 경우 사용
      v-if : 조건에 맞는 엘리먼트만 판별하여 렌더링해줌
    -->
    <div v-if="toggle">true</div>
    <div v-else>false</div>
    <button @click="onToggle">toggle</button>
    <div class="container">
      <h2>To-Do List</h2>  
      <form @submit.prevent="onSubmit">
        <div class="d-flex">
          <div class="flex-grow-1 mr-2">
            <input 
              class="form-control"
              type="text"
              placeholder="Type new to do"
              v-model="todo"
            >
          </div>   
          <div>
            <button
              class="btn btn-primary"
              type="submit"
            >
              Add
            </button>
          </div>
        </div>      
        <div v-if="hasError" style="color: red">
          This field cannot be empty.
        </div>
      </form>
      <div v-if="!todos.length">
        추가된 Todo가 없습니다.
      </div>  
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
              v-model="todo.completed"
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
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'; 

export default {
  setup() {
    const toggle = ref(false);
    const todo = ref('');
    const todos = ref([]);
    const hasError = ref(false);
    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gray'
    };

    const onSubmit = () => {
      if(todo.value === ''){
        hasError.value = true;
      } else {
      todos.value.push({
        id: Date.now(),
        subject: todo.value,
        completed: false,
        });
        hasError.value = false;
        todo.value = '';
      }
    }; 

    const onToggle = () => {
      toggle.value = !toggle.value
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    return {
      todo,
      todos,
      onSubmit,
      toggle,
      onToggle,
      hasError,
      todoStyle,
      deleteTodo,
    }

  }
}
</script>

<style>
  .todo {
    color: gray;
    text-decoration: line-through;
  }
</style>