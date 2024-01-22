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
          {{ todo.subject }}
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
    const todos = ref([
      {id : 1, subject : '휴대폰사기'},
      {id : 2, subject : '장보기'},
    ]);
    const hasError = ref(false);

    const onSubmit = () => {
      if(todo.value === '') {
        hasError.value = true;
      } else {
        todos.value.push({
          id: Date.now(),
          subject: todo.value,
        });
        hasError.value = false;
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
    }
  }
}
</script>

<style>
  .todo {
    color: blue;
  }
</style>
