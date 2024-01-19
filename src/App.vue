<template>
<div class="name">{{ name }}</div>
<div>{{ age }}</div>
<button
  class="btn btn-primary"
  v-on:click="updateName"
>
Click
</button>
</template>

<script>
/* 
! 새롭게 정의된 ref
Vue3 이전에는 뷰 템플릿의 DOM 또는 컴포넌트를 가리키는 속성이었지만, Vue3에서는 reactive reference를 의미합니다.

# ref 사용하는 방법
1. < script > 에 import { ref } from "vue";을 임포트합니다.
2. 변수를 선언하고 값을 ref()로 감싸줍니다.
3. 변수 값을 변경할 때 변수.value에 변경할 값을 넣어줍니다.

# ref의 장점
1. 타입 제한 없이 사용 가능함
2. 템플릿에서 단일 변수로 사용할 수 있음

-----------------------------------------------------------------------

# reactive 사용하는 방법
1.<script>에 import { reactive } from "vue";을 임포트합니다.
2.변수를 선언하고 값을 reactive()로 감싸줍니다.
3.property 값을 변경할 때 변수.property에 변경할 값을 넣어줍니다.

# 주의할 점
reactive는 object, array 이외에는 사용할 수 없습니다.

# reactive의 장점
1.자바스크립트와 템플릿 사이에 일관성이 있음
2.반응형 변수를 많이 선언할 때 간단하게 사용 가능
3.Vue2의 data()와 비슷함
*/

import {reactive} from 'vue';
import {ref} from 'vue';

export default {
  setup() {
    const name = reactive({
      id : 1
    });
    const age = ref(24);

    // const greeting = (name) => {
    //   return 'Hello' + name;
    // }

    const updateName = () => {
      name.id = 2
      age.value = 11
    }
    return {
      updateName,
      name,
      age,
    }
  }
}
</script>

<style>
  .name {
    color: blue;
  }
</style>
