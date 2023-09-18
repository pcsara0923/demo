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