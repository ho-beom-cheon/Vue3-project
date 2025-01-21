<template>
    <div>TodoComponent</div>
</template>

<script>
    import { useRoute } from 'vue-router'; 

    export default {
        setup() {
            const route = useRoute();
            
            console.log("route : " + route.name);
        }
    }

</script>

<style>
</style>