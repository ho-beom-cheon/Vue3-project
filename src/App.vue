<template>
  <div class="container">
    <h4>count : {{ count }}</h4>
    <h4>doubleCountComputed : {{ doubleCountComputed }}</h4>
    <h4>doubleCountComputed : {{ doubleCountComputed }}</h4>
    <h4>doubleCountMethod : {{ doubleCountMethod() }}</h4>
    <h4>doubleCountMethod : {{ doubleCountMethod() }}</h4>
    <button @click="count++">Add One</button>
    <h2>To-Do List</h2>
    <TodoSimpleForm @add-todo="addTodo"/>    
    <div v-if="!todos.length">
      추가된 ToDo가 없습니다.
    </div>
    <TodoList 
      :todos="todos" 
      @toggle-todo="toggleTodo" 
      @delete-todo="deleteTodo"
    />
  </div>
</template>

<script>
import {ref, computed} from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';

export default {
  components : {
    TodoSimpleForm,
    TodoList
  },
  setup() {
    const toggle = ref(false);
    
    const todos = ref([]);
    
    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gray',
    };


    const addTodo = (todo) => {
      console.log(todo);
      todos.value.push(todo);
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    const toggleTodo = (index) =>{
      console.log(todos.value[index]);
      todos.value[index].completed = !todos.value[index].completed; 
      console.log(todos.value[index]);
    }

    // # computed와 method의 차이점
    // 1. method는 인자값을 받을 수 있다.
    // 2. computed는 리액티브 스테이트 값이 변경될때만 값을 변경 시킬 수 있다.
    // 3. computed는 계산 된 값을 저장하고 있어서 여러번 호출하여도 계산되어 있는 값의 변경이 없으면 다시 computed를 수행하는게 아닌 저장된 값을 출력해준다.
    //    (테스트로 method와 computed에 로그를 찍은 뒤 수행하면 computed는 두번 호출함에도 한번만 찍히고 method는 호출하는 만큼 찍히게 된다.)
    const count = ref(1);
    const doubleCountComputed = computed(() => {
      console.log("computed");
      return count.value * 2;
    })

    const doubleCountMethod = () => {
      console.log("method");
      return count.value * 2;
    }

    return {
      deleteTodo,
      toggleTodo,
      addTodo,
      todos,
      toggle,
      todoStyle,
      doubleCountComputed,
      doubleCountMethod,
      count,
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
