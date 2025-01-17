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
    <hr />
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item" v-if="currentPage !== 1">
          <a style="cursor: pointer" class="page-link" @click="getTodos(currentPage - 1)">Previous</a>
        </li>
        <li 
            v-for="page in numberOfPages"
            :key="page"
            class="page-item"
            :class="currentPage === page ? 'active' : ''"
            >
          <a style="cursor: pointer" class="page-link" @click="getTodos(page)">{{ page }}</a>
        </li>
        <li class="page-item" v-if="numberOfPages !== currentPage">
          <a style="cursor: pointer" class="page-link" @click="getTodos(currentPage + 1)">Next</a>
        </li>
      </ul>
    </nav>
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
    const todos = ref([]);  
    const error = ref('');

    const numberOfTodos = ref(0);
    const limit = 5;
    const currentPage = ref(1);

    const numberOfPages = computed(() => {
        return Math.ceil(numberOfTodos.value/limit);
    });

    // ToDo 리스트 조회 선언
    const getTodos = async (page = currentPage.value) => {
      currentPage.value = page;
      try{
        const res = await axios.get(
          `http://localhost:3000/todos?_page=${page}&_limit=${limit}`
        );
        numberOfTodos.value = res.headers['x-total-count'];
        todos.value = res.data;
      } catch(e){
        console.log(e);
        error.value = '수행 중 [' + e + '] 오류가 발생했습니다.';
      }
    }

    // ToDo 리스트 조회 호출
    getTodos();

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
        error.value = '수행 중 [' + e + '] 오류가 발생했습니다.';
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
        error.value = '수행 중 [' + e + '] 오류가 발생했습니다.';
      }
    }

    // ToDo 리스트 Check Box toggle
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
        error.value = '수행 중 [' + e + '] 오류가 발생했습니다.';
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
      searchText,
      filteredTodos,
      error,
      numberOfPages,
      currentPage,
      getTodos,
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
