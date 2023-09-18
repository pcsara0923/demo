<!-- <script setup>
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
  //filter 過濾
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
</template> -->
<script setup>
import { ref } from 'vue'

const userInput = ref("")
const data = ref({})
const search = ref(false)

const submit = async (input) => {
  try {
    const response = await fetch(`https://www.omdbapi.com/?i=tt3896198&apikey=42951327&t=${input}`)
    const dataList = await response.json()
    data.value = dataList
    console.log("電影清單")
    console.log(dataList)
  } catch (error) {
    console.error("發生錯誤：", error)
  } finally {
    search.value = true
  }
}

</script>

<template>
  <input type="text" v-model="userInput" />
  <input type="button" value="提交" @click="submit(userInput)" />
  <div class="data" v-if="search">
    <div class="img-container">
      <img class="img" :src="data.Poster" />
    </div>
    <figcaption class="info">
      <p class="title">電影名稱:{{data.Title}}</p>
       <p class="title">主要演員:{{data.Actors}}</p>
      <p class="plot">劇情大意:{{data.Plot}}</p>
      <p class="rating" v-for="rating in data.Ratings">
        {{rating.Source}}
        評分：{{rating.Value}}
      </p>
    </figcaption>
  </div>
</template>
<style>
  .data {
  display: grid;
  grid-template-columns:50% 50%;
  gap: 5%;
}
  .img-container{
    padding-top: 20px;
  }
.info{
  display: flex;
  flex-direction: column;
  height: 50%;
  padding-top: 20px;
}

  .info p{
  font-size:1.1rem;
  margin-bottom: 0.8rem;
  }
  
</style>