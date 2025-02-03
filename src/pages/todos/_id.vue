<template>
    <h1>To-Do Page</h1>
    <div v-if="loding">
        Loding...
    </div>
    <form 
        v-else
        @submit.prevent="onSave"
    >
        <div class="row">
            <div class="col-6">
                <div class="form-group">
                    <label>Subject</label>
                    <input v-model="todo.subject" type="text" class="form-control">
                </div>
            </div>
            <div class="col-6">
                <div clas="form-group">
                    <label>Status</label>
                    <div>
                        <button 
                            class="btn" 
                            type="button"
                            :class="todo.completed ? 'btn-success' : 'btn-danger'"
                            @click="toggleTodoStatus"
                        >
                            {{ todo.completed ? 'Completed' : 'InCompleted' }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <button type="submit" class="btn btn-primary">
            Save
        </button>
        <button 
            class="btn btn-outline-dark ml-2"
            @click="moveToTodoListPage"
        >
            Cancle
        </button>
    </form>
</template>

<script>
    import { useRoute } from 'vue-router'; 
    import axios from 'axios';
    import { ref } from 'vue';
import router from '@/router';

    export default {
        setup() {
            const route = useRoute();
            const todo = ref(null);
            const loding = ref(true);
            const todoId = route.params.id;

            const getTodo = async () => {
                const res = await axios.get(`http://localhost:3000/todos/${todoId}`);
                console.log(route.params.id);
                console.log(res);

                todo.value = res.data;
                loding.value = false;
            };

            const toggleTodoStatus = () => {
                todo.value.completed = !todo.value.completed;
            };

            const moveToTodoListPage = () => {
                router.push({
                    name: 'Todos'
                })
            };

            getTodo();

            const onSave = async () => {
                await axios.put(`http://localhost:3000/todos/${todoId}`,{
                    subject: todo.value.subject,
                    completed: todo.value.completed
                })
            };

            return{
                todo,
                loding,
                toggleTodoStatus,
                moveToTodoListPage,
                onSave,
            }
        }
    }

    
</script>

<style>
</style>