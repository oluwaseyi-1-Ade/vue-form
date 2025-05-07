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
          class="outline-none px-5 py-[10px] bg-blue-500 text-white rounded-[10px]"
        >
          Create Account
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

const router = useRouter()

const email = ref(null)
const password = ref('')

const handleSubmit = async () => {
  console.log(email, password)

  try {
    const response = await axios.post(
      'https://jali.johnabrahamtosin.workers.dev/api/v1/auth/login',
      { email: email.value, password: password.value },
    )
    console.log(response)

    const token = response.data.data.token
    console.log(token)

    sessionStorage.setItem('authToken', token)

    router.push('/dashboard')
  } catch (error) {
    console.error(error)
  }
}
</script>

<style scoped></style>
