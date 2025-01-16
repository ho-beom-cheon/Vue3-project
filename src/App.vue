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

    const addTodo = (todo) => {
      error.value = '';
      // 데이터베이스에 todo 저장
      axios.post('http://localhost:3000/todos', {
        subject: todo.subject,
        completed: todo.completed,
      }).then(res => {
        console.log(res);
        todos.value.push(res.data);
      }).catch(e => {
        console.log(e);
        error.value = '수행 중' + error.value + '오류가 발생했습니다.';
      })
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    const toggleTodo = (index) =>{
      console.log(todos.value[index]);
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


    // # computed와 method의 차이점
    // 1. method는 인자값을 받을 수 있다.
    // 2. computed는 리액티브 state 값이 변경될때만 값을 변경 시킬 수 있다.
    // 3. computed는 계산 된 값을 저장하고 있어서 여러번 호출하여도 계산되어 있는 값의 변경이 없으면 다시 computed를 수행하는게 아닌 저장된 값을 출력해준다.
    //    (테스트로 method와 computed에 로그를 찍은 뒤 수행하면 computed는 두번 호출함에도 한번만 찍히고 method는 호출하는 만큼 찍히게 된다.)
    // const count = ref(1);
    // const doubleCountComputed = computed(() => {
    //   console.log("computed");
    //   return count.value * 2;
    // })

    // const doubleCountMethod = () => {
    //   console.log("method");
    //   return count.value * 2;
    // }

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
