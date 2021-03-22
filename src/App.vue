<template>
  <div class = "flex flex-col items-center w-2/12 mx-auto">
    <h3 class="font-medium text-xl sm:w-56 lg:w-48 md-w-32">Masukkan Username Github Anda!</h3>
    <input v-model='name' class="ml-4 p-1 border-solid shadow-outline mt-4" placeholder="Github Username">
    <button
      class="border-red-700 mt-4 w-32 rounded-md p-2 mb-10 uppercase font-bold bg-indigo-400 shadow-xl"
      @click='newName = name'
      >Press Me</button>
  </div>
  <ul>
    <li v-for="lib in data" :key="lib.name">{{ lib.name }}</li>
  </ul>
</template>

<script>
import {ref, watch, reactive, toRefs} from 'vue';
export default {
  name: 'App',
  setup() {
    const name = ref(null);
    const newName = ref(null);
    const state = reactive({data: []});

    watch(() => {
      console.log("newName", newName.value);
      if(newName.value)
        fetch(`https://api.github.com/users/${newName.value}/repos`)
        .then((response) => response.json())
        .then((data) => {
          state.data = data;
          console.log('data',data);
          name.value = "";
        });
    })
    return {
      name,
      newName,
      ...toRefs(state)
    }
  }
  
}
</script>

<style>
#app {
    margin-top: 60px;
    color: #2c3e50;
    text-align: center;
    font-family: sans-serif, Arial;
}
.font-bold {
  color: white;
}
</style>
