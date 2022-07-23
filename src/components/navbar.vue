<script setup>
import { ref } from 'vue'
import Modal from "./Modal.vue"
import { database } from '../server'
    defineProps({
        colNav:"",
        ubahNav:false,
        isValid: false
})     
  
    
    const emits = defineEmits(['openModal'])
    const isModalShow = ref(false)
    const isAuthenticated = ref(false)
    const currentFullname = ref("")

    const openModalLogin = () => {
        emits('openModal')
        isModalShow.value = true
        isLogin.value = true
    }
    const openModalRegister = () => {
        emits('openModal')
        isModalShow.value = true
        isLogin.value = false
    }
    const isLogin = ref(false)

    const email = ref ("")
    const password = ref ("")
    const fullname = ref ("")
    const username = ref ("")
    const konpas = ref("")
    const isFormValid = () => {
      return email.value.includes("@") && email.value.length > 5 && password.value.length > 6
    }
    const isFormRegValid = () => {
      return email.value.includes("@") && email.value.length > 5 && password.value.length > 6 && konpas.value === password.value && fullname.value.length > 2
    }
    const submitLogin = () => {
      currentFullname.value = database.value.filter((x) => x.email === email.value).map((x) => x.fullname)[0];
      isModalShow.value = false
      isAuthenticated.value = true
      email.value = ""
      password.value = ""
    }
    const submitRegister = () => {
      const payload = {
        id: new Date(),
        username: username.value,
        email: email.value,
        fullname: fullname.value
      }
      database.value.push(payload)
      isModalShow.value = false
      email.value = ""
      password.value = ""
    }
    const isLogout = () => {
      currentFullname.value = ""
      isAuthenticated.value = false
    }
</script>


<template>
<header class="sticky top-0">
<nav :class="colNav" class="bg-white border-gray-200 px-2 sm:px-4 py-2.5 rounded dark:bg-gray-900">
  <div class="container flex flex-wrap justify-between items-center mx-auto">
    <a href="https://hp.com/" class="flex items-center">
        <img src="./hp.png" class="mr-3 h-6 sm:h-9" alt="Hp Logo">
    </a>
    <button data-collapse-toggle="navbar-default" type="button" class="inline-flex items-center p-2 ml-3 text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600" aria-controls="navbar-default" aria-expanded="false">
      <span class="sr-only">Open main menu</span>
      <svg class="w-6 h-6" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M3 5a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 15a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z" clip-rule="evenodd"></path></svg>
    </button>
    <div class="hidden w-full md:block md:w-auto" id="navbar-default">
      <ul class="flex flex-col mt-4 md:flex-row md:space-x-8 md:mt-0 md:text-sm md:font-medium">
        <li>
          <router-link to="/"><a href="#" class="block py-2 pr-4 pl-3 text-white bg-blue-700 rounded md:bg-transparent md:text-blue-700 md:p-0 dark:text-white" aria-current="page">Home</a></router-link>
        </li>
        <li>
          <router-link to="/About"><a href="#" class="block py-2 pr-4 pl-3 text-gray-700 border-b border-gray-100 hover:bg-gray-50 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700">About</a></router-link>
        </li>
        <div v-if="isAuthenticated">
        <li >
          <span @click="isLogout()" class="block py-2 pr-4 pl-3 text-gray-700 border-b border-gray-100 hover:bg-gray-50 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700">Logout</span>
        </li>
        <li>
          <span class="block py-2 pr-4 pl-3 text-gray-700 border-b border-gray-100 hover:bg-gray-50 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700">{{ currentFullname }}</span>
        </li>
        </div>
        
        <div v-else>
        <li >
          <span @click="openModalLogin()" class="block py-2 pr-4 pl-3 text-gray-700 border-b border-gray-100 hover:bg-gray-50 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700">Login</span>
        </li>
        <li>
          <a @click="openModalRegister()" class="block py-2 pr-4 pl-3 text-gray-700 border-b border-gray-100 hover:bg-gray-50 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700">Register</a>
        </li>
        </div>
      </ul>
    </div>
  </div>
</nav>
</header>
<Modal v-if="isModalShow" @close-modal="isModalShow = false"
:title="isLogin ? 'Login' : 'Register'"
:btnText="isLogin ? 'Login' : 'Register'" :isValid= "isLogin ? !isFormValid() : !isFormRegValid()"
@submit-modal="isLogin ? submitLogin() : submitRegister()">
<div class="w-full p-4">
  
<form>
  
  <div class="mb-6" v-if="!isLogin">
    <label for="text" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Masukan Fullname</label>
    <input v-model="fullname" type="text" id="text" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Masukan Fullname" required="">
  </div>
  <div class="mb-6" >
    <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Masukan Email</label>
    <input v-model="email" type="email" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Masukan Email" required="">
  </div>
  <div class="flex items-start mb-6">
  
  </div>

  <div class="mb-6">
    <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Masukan Password</label>
    <input v-model="password" type="password" id="password" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Masukan Password" required="">
  </div>
  <div class="mb-6" v-if="!isLogin">
    <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Konfirmasi Password</label>
    <input v-model="konpas" type="password" id="password" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Masukan ulang password" required="">
  </div>
  <div class="flex items-start mb-6">
    
  </div>
 
</form>

  <p class="text-center text-gray-500 text-xs">
    &copy;2022 HP Official Store. Indonesia.
  </p>
</div>
</Modal>
</template>