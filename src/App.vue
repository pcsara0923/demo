<script setup>
import { ref,computed } from 'vue'

//給每個todo一個id
let id = 0

const newTodoList = ref('')
const newTimeList = ref('')
const hideCompleted = ref(false)

const todos = ref([
  { uid: id++, text: 'coding',time:'8:00AM', done: true },
  { uid: id++, text: 'debug',time:'9:00AM', done: false  }
])

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})
   
function addTodo() {
  todos.value.push({ uid: id++, text: newTodoList.value,time: newTimeList.value, done: false })
  
  newTodoList.value = ''
  newTimeList.value =''
  done=''
}

function removeTodo(todo) {
 todos.value =  todos.value.filter((i) => i !== todo)
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTimeList" placeholder="時間"><br>
    <input v-model="newTodoList" placeholder="事情">
    <button>新增項目</button>    
  </form>
  <ol>
    <li v-for="todo in filteredTodos" :key="todo.uid">
     <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.time }} {{todo.text}}</span>
      <button @click="removeTodo(todo)" style="color:red">x</button>
    </li>
  </ol>
   <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? '顯示全部' : '隱藏完成選項' }}
  </button>
</template>