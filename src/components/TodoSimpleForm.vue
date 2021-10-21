<template>
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
</template>
<script>
import { ref } from 'vue';

    export default {
        emits: ['add-todo'],
        setup(props, context) {
            const todo = ref('');
            const hasError = ref(false);

            const onSubmit = () => {
                if(todo.value === ''){
                    hasError.value = true;
                } else {
                    context.emit('add-todo', {
                        id: Date.now(),
                        subject: todo.value,
                        completed: false,
                    }); // 자식컴포넌트 -> 부모컴포넌트
                    hasError.value = false;
                    todo.value = '';
                }
            }; 
            return {
                todo,
                hasError,
                onSubmit,
            };
        }
    }
</script>
<style>
</style>