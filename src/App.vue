<template>
  <div class="container">
    <h2>To-Do List</h2>
    <input 
      class="form-control"
      type="text" 
      v-model="searchText"
      placeholder="Search"
    >
    <hr />
    <TodoSimpleForm @add-todo="addTodo"/>    
    <div style="color: red;">{{ error }}</div>
    <div v-if="!filteredTodos.length">
      There is nothing to display
    </div>
    <TodoList 
      :todos="filteredTodos" 
      @toggle-todo="toggleTodo" 
      @delete-todo="deleteTodo"
    />
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';
import axios from 'axios';

export default {
  components : {
    TodoSimpleForm,
    TodoList
  },
  setup() {
    const toggle = ref(false);
    
    const todos = ref([]);
    
    const error = ref('');

    const addTodo = async (todo) => {
      // 데이터베이스에 todo 저장
      error.value = '';

      try{
        const res = await axios.post('http://localhost:3000/todos', {
          subject: todo.subject,
          completed: todo.completed,
        });
        todos.value.push(res.data);

      } catch(e){
        console.log(e);
        error.value = '수행 중' + e + '오류가 발생했습니다.';
      }
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    const toggleTodo = (index) =>{
      todos.value[index].completed = !todos.value[index].completed; 
      console.log(todos.value[index]);
    }

    const searchText = ref('');
    const filteredTodos = computed(() => {
        if(searchText.value) {
          return todos.value.filter(todo => {
             return todo.subject.includes(searchText.value);
          });
        }

        return todos.value;
    });


    return {
      deleteTodo,
      toggleTodo,
      addTodo,
      todos,
      toggle,
      searchText,
      filteredTodos,
      error,
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
