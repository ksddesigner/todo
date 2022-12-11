<script setup>
import {onMounted, ref, watch} from "vue"

const inputEl = ref()
const todos = ref([])

const task = ref("")

const addTodo = () => {
  todos.value.unshift({
    task: task.value,
    status: false,
  })

  task.value = ""

  inputEl.value.focus()
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const toggleTaskStatus = (index) => {
  todos.value[index].status = !todos.value[index].status
}

const clearList = () => {
  todos.value = []
}

watch(todos, (value) => {
  window.localStorage.setItem("todos", JSON.stringify(value))
},{deep: true})

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || []
})
</script>

<template>
  <div class="h-screen flex items-center justify-center">
    <div class="max-w-[700px] w-[700px] space-y-4">

      <!--Input-->
      <div class="flex items-center space-x-2 p-4 rounded-xl bg-gray-700">
        <input type="text"
               v-model="task"
               ref="inputEl"
               placeholder="Type your task"
               class="w-full px-4 py-2 rounded-lg bg-gray-700 border border-gray-500 text-white hover:border-gray-300 focus:border-white focus:outline-none"
               @keydown.enter="addTodo">

        <button class="px-4 py-2 rounded-lg bg-purple-600 text-white hover:bg-purple-500 focus:bg-purple-500 focus:outline-none whitespace-nowrap disabled:opacity-30 disabled:bg-purple-600"
                :disabled="task === ''"
                @click="addTodo">
          Create Task
        </button>
      </div>

      <!--List-->
      <div class="p-4 rounded-xl bg-gray-700 space-y-3" v-if="todos.length">
        <div v-for="(i, index) in todos" class="flex items-center space-x-4">
          <div class="w-[32px] aspect-[1] p-1  rounded-lg transition cursor-pointer"
               :class="i.status ? 'bg-purple-500 hover:bg-purple-400' : 'bg-gray-600 hover:bg-gray-500'"
               @click="toggleTaskStatus(index)">
            <img src="@/assets/img/ok.svg" alt="">
          </div>

          <div class="truncate flex-1" :class="i.status ? 'text-gray-400 line-through' : 'text-white'">{{i.task}}</div>

          <div class="w-[32px] aspect-[1] rounded-lg bg-gray-600 p-1 hover:bg-red-500 cursor-pointer transition"
               @click="removeTodo(index)">
            <img src="@/assets/img/delete.svg" alt="">
          </div>
        </div>
      </div>

      <div class="px-3 py-1 rounded-full bg-gray-700 text-gray-300 text-sm hover:bg-red-500 hover:text-white transition w-fit mx-auto cursor-pointer"
           @click="clearList"
           v-if="todos.length">
        Clear all
      </div>
    </div>
  </div>

</template>

<style scoped>

</style>
