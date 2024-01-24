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
    <form @submit.prevent="onSubmit">
      <div class="d-flex">
        <div class="flex-grow-1 mr-2">
          <input 
            class="form-control"
            type="text" 
            v-model="todo"
            placeholder="Type new to-do"
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
      <div v-show="hasError" style="color: red;">
        This field cannot be empty
      </div>
    </form>
    <div
      v-for="todo in todos"
      :key="todo.id"
      class="card mt-2"
      >
      <div class="card-body p-2">
        <div class="form-check">
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
          
      </div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';

export default {
  setup() {
    const toggle = ref(false);
    const todo = ref('');
    const todos = ref([]);
    const hasError = ref(false);
    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gray',
    };

    const onSubmit = () => {
      if(todo.value === '') {
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
    }


    const onToggle = () => {
      toggle.value = !toggle.value;
    }

    return {
      onSubmit,
      onToggle,
      hasError,
      todos,
      todo,
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
