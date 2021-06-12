<template>
  <div class = "flex flex-col items-center w-11/12 mx-auto">
  <!-- notification -->
    <div class="Notification" v-if="notificationMessage">
      <div class="bg-teal-100 border-t-4 border-teal-500 rounded-b text-teal-900 px-4 py-3 shadow-md mb-4" role="alert">
        <div class="flex">
          <div class="py-1"><svg class="fill-current h-6 w-6 text-teal-500 mr-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M2.93 17.07A10 10 0 1 1 17.07 2.93 10 10 0 0 1 2.93 17.07zm12.73-1.41A8 8 0 1 0 4.34 4.34a8 8 0 0 0 11.32 11.32zM9 11V9h2v6H9v-4zm0-6h2v2H9V5z"/></svg></div>
          <div>
            <p class="font-bold text-blue-900">successfully loaded..</p>
            <p class="text-sm">thank you for using our application.</p>
          </div>
        </div>
      </div>
    </div>

    <h3 class="font-medium text-xl sm:w-56 lg:w-full md-w-32">Masukkan Username Github Anda!</h3>
    <input v-model="name" class="p-2 mt-4 placeholder-gray-600 border-transparent text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-700 border border-gray-400 focus:border-blue-500 rounded-lg focus:outline-none" placeholder="Github Username">
    <button
      class="mt-4 w-32 rounded-lg focus:outline-none p-3 mb-10 uppercase font-bold bg-indigo-400 shadow-md"
      @click="submit"
      >Press Me</button>
  </div>
  <!-- <ul>
    <li v-for="lib in data" :key="lib.name">{{ lib.name }}</li>
  </ul> -->
  <div v-for="lib in data" :key="lib.name" class="h-auto text-left mx-auto w-10/12 py-5 px-5 m-4 rounded-xl border border-gray-200 bg-white shadow-md">
    <ul>
      <li><p><span class="text-blue-700">Author</span>: {{lib.owner.login}}</p></li>
      <li><p><span class="text-blue-700">Repository:</span> {{lib.name}}</p></li>
      <li><p><span class="text-blue-700">Languages:</span> {{lib.language}}</p></li>
    </ul>
  </div>
</template>

<script lang="ts">
import {defineComponent} from 'vue';
import {ref, watch, reactive, toRefs} from 'vue';
export default defineComponent({
  name: 'App',
  setup() {
    const name = ref(null);
    const notificationMessage = ref(false);
    const state = reactive({data: []});

    watch(name, () => notificationMessage.value = false)
    const submit = () => {
      fetch(`https://api.github.com/users/${name.value}/repos`)
      .then((response) => response.json())
      .then((data) =>  {
        notificationMessage.value = true;
        state.data = data;
        console.log('data', data);
      });
    }
    return {
      name,
      submit,
      notificationMessage,
      ...toRefs(state)
    }
  }
});
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
