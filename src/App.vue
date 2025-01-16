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

    // ToDo 리스트 조회 선언
    const getTodo = async () => {
      error.value = '';

      try{
        const res = await axios.get('http://localhost:3000/todos');
        console.log(res);
        todos.value = res.data;
      } catch(e){
        console.log(e);
        error.value = '수행 중' + e + '오류가 발생했습니다.';
      }
    }

    // ToDo 리스트 조회 호출
    getTodo();

    // ToDo 리스트 등록 저장
    const addTodo = async (todo) => {
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

    // ToDo 리스트 내용 삭제
    const deleteTodo = async (index) => {
      error.value = '';
      const id = todos.value[index].id;
      try{
        const res = await axios.delete('http://localhost:3000/todos/' + id);
        console.log(res);
        todos.value.splice(index, 1);
      } catch(e){
        console.log(e);
        error.value = '수행 중' + e + '오류가 발생했습니다.';
      }
    }

    const toggleTodo = async (index) => {
      error.value = '';
      const id = todos.value[index].id;
      try{
        const res = await axios.patch('http://localhost:3000/todos/' + id,{
          completed: !todos.value[index].completed,
        });
        console.log(res);
        todos.value[index].completed = !todos.value[index].completed; 
      } catch(e){
        console.log(e);
        error.value = '수행 중' + e + '오류가 발생했습니다.';
      }
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
