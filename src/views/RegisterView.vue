<script setup>

import axios from 'axios'
import { reactive } from 'vue'

const state = reactive({
  name: '',
  email: '',
  password: ''
})

// Send header access json
const register = async (event) => {
  event.preventDefault()
  axios.defaults.headers.common['Accept'] = 'application/json'
  try {
    const response = await axios.post('http://localhost:8000/api/auth/register', {
      name: state.name,
      email: state.email,
      password: state.password
    })

    if (response.status == 200) {
      setTimeout(() => {
        window.location.href = '/'
      }, 2000)
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
      <form @submit="register">
        <img class="rounded-full h-20 w-20 mx-auto" src="/img/perfil.svg" />
        <h2 class="title">Register</h2>
        <div class="input-div one">
          <div class="i">
            <i class="fas fa-user"></i>
          </div>
          <div class="div">
            <input type="text" class="input" placeholder="User Name" v-model="state.name" required>
          </div>
        </div>
        <div class="input-div one">
          <div class="i">
            <i class="fas fa-user"></i>
          </div>
          <div class="div">
            <input type="text" class="input" placeholder="correo@correo.com" v-model="state.email" required>
          </div>
        </div>
        <div class="input-div pass">
          <div class="i">
            <i class="fas fa-lock"></i>
          </div>
          <div class="div">
            <input type="password" class="input" placeholder="Password" v-model="state.password" required>
          </div>
        </div>
        <button @submit="register" class="btn">Register</button>
      </form>
    </div>
  </div>
</template>