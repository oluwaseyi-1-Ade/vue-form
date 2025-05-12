<template>
  <div>
    <div>
      <h1 className="font-bold text-black text-[30px] lg:text-[45px]">Welcome Back!</h1>
      <p className="font-normal text-sm text-gray-500">
        Kindly fill in your credentials to login into your account.
      </p>
    </div>

    <form @submit.prevent="handleSubmit">
      <div className="mt-10">
        <label className="font-Inter font-normal text-sm text-black">Email Address</label><br />
        <input
          type="email"
          placeholder="name@domain.com"
          v-model="email"
          className="w-full font-normal text-black text-sm underline border border-gray outline-none rounded-[5px] px-[21px] py-[12px]"
        />
      </div>

      <div className="mt-10">
        <label className="font-Inter font-normal text-sm text-black">Confirm Password</label>
        <br />
        <div
          className="flex items-center gap-2 justify-between rounded-[5px] mt-2 border border-gray px-2 lg:px-[21px] py-[12px]"
        >
          <input
            type="password"
            placeholder="Enter password"
            v-model="password"
            className="w-full text-black outline-none font-Inter font-normal text-sm"
          />
        </div>
      </div>

      <div class="w-fit mx-auto mt-5">
        <button
          type="submit"
          :disabled="isLoading"
          class="btn outline-none px-5 py-[10px] bg-blue-500 text-white rounded-[10px]"
        >
          Login
        </button>
      </div>
    </form>

    <div class="flex items-center gap-3 mt-4 justify-center">
      <p class="text-black">Does not have an account?</p>
      <router-link to="/" class="text-base text-blue-500 underline decoration-blue-500"
        >Signup</router-link
      >
    </div>
  </div>
</template>

<script setup>
import axios from 'axios'
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useToast } from 'vue-toastification'

const router = useRouter()
const toast = useToast()

const email = ref('')
const password = ref('')
const isLoading = ref(false)

const handleSubmit = async () => {
  isLoading.value = true
  try {
    if (email.value === '' || password.value === '') {
      isLoading.value = false
      toast.error('Kindly fill all inputs correctly')
      return
    }

    console.log(email.value, password.value)
    const response = await axios.post(
      'https://jali.johnabrahamtosin.workers.dev/api/v1/auth/login',
      { email: email.value, password: password.value },
    )
    console.log(response)
    email.value = ''
    password.value = ''
    const token = ref(response.data.data.token)
    console.log(token.value)

    sessionStorage.setItem('authToken', token.value)
    router.push('/dashboard')
    toast.success('Login successful')
  } catch (error) {
    console.error(error)
    toast.error(error.response.data.message || 'Login failed')
  }
  isLoading.value = false
}
</script>

<style scoped>
.btn:disabled {
  opacity: 0.5;
  background-color: #ccc;
  cursor: not-allowed;
  transition: opacity 0.3s ease;
}
</style>
