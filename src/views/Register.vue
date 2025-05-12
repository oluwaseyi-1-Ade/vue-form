<template>
  <div>
    <div>
      <h1 className="font-bold text-black text-[30px] lg:text-[48px]">Welcome Back!</h1>
      <p className="font-normal text-sm text-gray-500">
        Kindly fill in your details to create your account.
      </p>
    </div>
    <form @submit.prevent="handleSubmit" class="">
      <div className="md:mt-6 pt-5 sm:flex justify-between items-center gap-3">
        <div className="w-full">
          <label className="font-Inter font-normal text-sm text-black">Full Name</label> <br />
          <input
            v-model="formValues.fullName"
            type="text"
            placeholder="Enter your name"
            className="w-full text-black outline-none font-Inter font-normal text-xs mt-2 rounded-[5px] border px-2 lg:px-[21px] py-[12px] border-gray"
          />
        </div>
        <div className="w-full">
          <label className="font-Inter font-normal text-sm text-black">Email Address</label>
          <br />
          <input
            type="email"
            v-model="formValues.email"
            placeholder="Enter your email address"
            className="w-full text-black outline-none font-Inter font-normal text-xs mt-2 rounded-[5px] border px-2 lg:px-[21px] py-[12px] border-gray"
          />
        </div>
      </div>
      <div className="md:mt-6 sm:flex justify-between items-center gap-3">
        <div className="w-full">
          <label className="font-Inter font-normal text-sm text-black">Business Name</label>
          <br />
          <input
            v-model="formValues.businessName"
            type="text"
            placeholder="Enter your business name"
            name="businessName"
            className="w-full text-black outline-none font-Inter font-normal text-xs mt-2 rounded-[5px] border px-2 lg:px-[21px] py-[12px] border-gray"
          />
        </div>
        <div className="w-full">
          <label className="font-Inter font-normal text-sm text-black">Phone Number</label>
          <br />
          <input
            v-model="formValues.phone"
            type="text"
            placeholder="Enter your phone number"
            name="phone"
            className="w-full text-black outline-none font-Inter font-normal text-xs mt-2 rounded-[5px] border px-2 lg:px-[21px] py-[12px] border-gray"
          />
        </div>
      </div>

      <div className="md:mt-6 sm:flex justify-between items-center gap-3">
        <div className="w-full">
          <label className="font-Inter font-normal text-sm text-black">Password</label> <br />
          <div class="w-full rounded-[5px] mt-2 border border-gray px-2 lg:px-[21px] py-[12px]">
            <input
              v-model="formValues.password"
              type="password"
              placeholder="Enter password"
              name="password"
              className="w-full text-black outline-none font-Inter font-normal text-xs"
            />
          </div>
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
      <p class="text-black">Already have an account?</p>
      <router-link to="/login" class="text-base text-blue-500 underline decoration-blue-500"
        >Login</router-link
      >
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'
import { useToast } from 'vue-toastification'

const router = useRouter()
const toast = useToast()

const formValues = ref({
  email: '',
  password: '',
  phone: '',
  fullName: '',
  businessName: '',
})

const handleSubmit = async () => {
  console.log(formValues.value)
  try {
    const response = await axios.post(
      'https://jali.johnabrahamtosin.workers.dev/api/v1/auth/signup',
      formValues.value,
      {
        headers: {
          'Content-Type': 'application/json',
          accept: 'application/json',
        },
      },
    )
    console.log(response.data)
    toast.success('Registration successful')
    router.push('/login')

    formValues.value = {
      email: '',
      password: '',
      phone: '',
      fullName: '',
      businessName: '',
    }
  } catch (error) {
    console.error(error.response.data.message, 'here')

    toast.error(error.response.data.message[0] || 'Registration failed')
  }
}
</script>

<style scoped></style>
