<script setup>

import axios from 'axios'
import { reactive } from 'vue'

const state = reactive({
  email: '',
  password: ''
})


const login = async (event) => {
  event.preventDefault()
  axios.defaults.headers.common['Accept'] = 'application/json'
  try {
    const response = await axios.post('http://localhost:8000/api/auth/login', {
      email: state.email,
      password: state.password
    })
    console.log(response)

    if (response.status == 200) {

      if (localStorage.getItem('token') && localStorage.getItem('name')) {
        localStorage.removeItem('token')
        localStorage.removeItem('name')
      }

      localStorage.setItem('token', response.data.token)
      localStorage.setItem('name', response.data.name)
      setTimeout(() => {
        window.location.href = '/restaurant'
      }, 1000)
    }

    // console.log(response)
  } catch (error) {
    console.error(error)
  }
}


</script>

<template>
  <img class="wave" src="/img/wave.png">
  <div class="container">
    <div class="img">
      <img src="/img/restaurant.svg">
    </div>
    <div class="login-content">
      <form @submit="login">
        <img class="rounded-full h-20 w-20 mx-auto" src="/img/perfil.svg" />
        <h2 class="title">Login</h2>
        <div class="input-div one">
          <div class="i">
            <i class="fas fa-user"></i>
          </div>
          <div class="div">
            <input type="email" class="input" v-model="state.email" placeholder="correo@correo.com">
          </div>
        </div>
        <div class="input-div pass">
          <div class="i">
            <i class="fas fa-lock"></i>
          </div>
          <div class="div">
            <input type="password" class="input" v-model="state.password" placeholder="password">
          </div>
        </div>
        <RouterLink to="/register">Register</RouterLink>
        <button @submit="login" class="btn">Login</button>
      </form>
    </div>
  </div>
</template>

<style scoped></style>
