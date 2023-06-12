<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'

const restaurants = ref([])
const name = localStorage.getItem('name')

const getRestaurants = async () => {
  axios.defaults.headers.common['Authorization'] = 'Bearer ' + localStorage.getItem('token')
  try {
    const response = await axios.get('http://localhost:8000/api/restaurants')
    console.log(response.data.data)
    restaurants.value = response.data.data
  } catch (error) {
    console.error(error)
  }
}

const logout = () => {
  axios.defaults.headers.common['Authorization'] = 'Bearer ' + localStorage.getItem('token')
  try {
    const response = axios.post('http://localhost:8000/api/auth/logout')
    console.log(response)
      localStorage.removeItem('token')
      localStorage.removeItem('name')
      setTimeout(() => {
        window.location.href = '/'
      }, 1000)
  } catch (error) {
    console.log(error)
  }
}


if(localStorage.getItem('token')){
    onMounted(() => {
    getRestaurants()
  })
}else{
  window.location.href = '/'
}

</script>


<template>
  <div class="flex flex-col items-center justify-center my-6">
    <h1 class="text-3xl font-bold my-3"><span class="">Bienvenido</span> {{ name }}</h1>
    <button @click="logout"
      class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded my-3">Logout</button>
    <h2 class="text-2xl font-bold mb-4">Estos son los restaurantes disponibles</h2>
    <div class="flex flex-wrap justify-center">
      <div v-for="restaurant in restaurants" :key="restaurant.id" class="max-w-sm rounded overflow-hidden shadow-lg m-4">
        <img class="w-full" :src="restaurant.image_path" alt="Sunset in the mountains">
        <div class="px-6 py-4">
          <div class="font-bold text-3xl mb-2 text-center">{{ restaurant.name }}</div>
          <p class="text-gray-700 text-base">
            {{ restaurant.description }}
          </p>
        </div>
        <div class="px-6 pt-4 pb-2">
          <span
            class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">Dishes:</span>
          <span v-for="dish in restaurant.dishes" :key="dish.id"
            class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{
              dish.name }}</span>
        </div>
      </div>
    </div>
  </div>
</template>