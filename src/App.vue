<template>
  <div>
    <!-- 
      v-show : 모든 엘리먼트를 렌더링 한 후 판별 (초기 렌더 비용이 많이 듬)
               자주 렌더링이 되어야 하는 경우 사용
      v-if : 조건에 맞는 엘리먼트만 판별하여 렌더링해줌
    -->
    <div class="container">
      <h4>count: {{ count }}</h4>
      <h4>doubleCountComputed: {{ doubleCountComputed }}</h4>
      <h4>doubleCountMethod: {{ doubleCountMethod() }}</h4>
      <button @click="count++">Plus One</button>
      <h2>To-Do List</h2>  
       <input 
          class="form-control"
          type="text"
          placeholder="search"
          v-model="searchText"
        >
        <hr/>
      <TodoSimpleForm @add-todo="addTodo"/>
      <div v-if="!filterdTodos.length">
        There is nothing to display.
      </div>  
      <TodoList 
        :todos="filterdTodos" 
        @toggle-Todo="toggleTodo"
        @delete-todo="deleteTodo"
        /> <!-- 부모컴포넌트 -> 자식 컴포넌트로 데이터 이동 -->
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'; 
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';

export default {
  components: {
    TodoSimpleForm,
    TodoList,
  },
  setup() {
    const toggle = ref(false);
    const todos = ref([]);
    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gray'
    };

    const toggleTodo = (index) => { // 자식 컴포넌트에서 받은 데이터가 들어옴
      todos.value[index].completed = !todos.value[index].completed;
    }

    const addTodo = (todo) => { // 자식 컴포넌트에서 받은 데이터가 들어옴
      todos.value.push(todo);
    }; 

    const onToggle = () => {
      toggle.value = !toggle.value
    }

    const deleteTodo = (index) => { // 자식 컴포넌트에서 받은 데이터가 들어옴
      todos.value.splice(index, 1);
    }

    const count = ref(1);
    const doubleCountComputed = computed(() => { // 값을 캐시함
      return count.value * 2; 
    });

    const doubleCountMethod = () => { // 매개변수 받을 수 있음
      return count.value * 2;
    }

    const searchText = ref('');
    const filterdTodos = computed(() => {
      if(searchText.value){
        return todos.value.filter(todo => {
          return todo.subject.includes(searchText.value);
        });
      }
      return todos.value;
    })

    return {
      todos,
      addTodo,
      toggle,
      onToggle,
      todoStyle,
      deleteTodo,
      toggleTodo,
      count,
      doubleCountComputed,
      doubleCountMethod,
      searchText,
      filterdTodos,
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