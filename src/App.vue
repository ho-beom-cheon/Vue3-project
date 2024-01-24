<template>
  <!-- v-if 와 v-show 
    v-if는 일반적으로 토글하는데 비용이 많이 든다
    v-if의 조건이 런타임 동안에 거의 바뀌지 않을 경우에 사용하면 된다.
    v-show는 두개의 div를 전부 렌더링 해야해서 초기 렌더링 비용이 많이 든다.
  -->
  <!-- <div v-show="toggle">true</div>
  <div v-show="!toggle">false</div> -->
  <!-- <div v-if="toggle">true</div>
  <div v-else>false</div> -->
  <!-- <button @click="onToggle">Toggle</button> -->

  <div class="container">
    <h2>To-Do List</h2>
    <TodoSimpleForm @add-todo="addTodo"/>
    
    <div v-if="!todos.length">
      추가된 ToDo가 없습니다.
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
            class="form-check-labal" 
            :class="{ todo : todo.completed }"
          >
            {{ todo.subject }}
          </label>
        </div>
        <div>
          <button 
            class="btn btn-danger btn-sm"
            @click="deleteTodo(index)"
          >Delete</button>
        </div>  
      </div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';

export default {
  components : {
    TodoSimpleForm
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

    const onToggle = () => {
      toggle.value = !toggle.value;
    }

    return {
      onToggle,
      deleteTodo,
      addTodo,
      todos,
      toggle,
      todoStyle,
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
