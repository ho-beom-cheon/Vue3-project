<template>
  <input 
    type="text" 
    v-model="name"
  >
  <button
    class="btn btn-primary"
    @click="onSubmit"
  >
    Click
  </button>
  <div>
    {{ name }}
  </div>
</template>

<script>
import {ref} from 'vue';

export default {
  setup() {
    const name = ref('hoho');

    const onSubmit = () => {
      console.log(name.value);
    }
    // const updataName = (e) => {
    //   name.value = e.target.value;
    // }
    return {
      onSubmit,
      // updataName,
      name,
    }
  }
}
</script>

<style>
  .name {
    color: blue;
  }
</style>
