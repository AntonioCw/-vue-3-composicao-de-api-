<template>
  <h1>
    Lista de tarefas
    <router-link :to="{name: 'todos.create'}">+</router-link>

  </h1>

  <div v-if="loading">
    Carregando as tarefas
  </div>

  <ul>
    <li v-for="todo in todos" :key="todo.identify">
      <todo :todo="todo"></todo>
    </li>
  </ul>
  <input type="text" v-model="name">
</template>

<script>
import {onMounted, ref} from 'vue'

import TodoService from '@/services/todos.service'
import Todo from './Todo'

export default {
    name: 'Todos',
    setup() {
      const todos = ref([])

      const loading = ref(false)

        onMounted( () => {
          loading.value = true

          TodoService.getAll()
              .then(response => todos.value = response.data.data)
              .catch(error => console.log(error))
              .finally(() => loading.value = false)
        })

      return {
        loading,
        todos,
      }
    },
  components: {
      Todo
  }
}
</script>