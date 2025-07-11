<template>
  <div class="min-h-screen flex bg-gray-900 text-white">
    <div
      class="flex flex-col md:flex-row w-full shadow-lg bg-white text-gray-900 overflow-hidden"
    >
      <!-- Left Panel -->
      <div
        class="hidden md:flex flex-col items-center justify-center w-1/2 bg-purple-100 p-8"
      >
        <div class="w-full flex items-start">
          <img :src="Logo" alt="Team Achieve Logo" class="mb-4" />
        </div>
        <img
          :src="BackgoundImage"
          alt="Happy people"
          class="w-full h-auto rounded-lg"
        />
        <div class="my-5 text-center">
          <h2 class="text-xl text-[#61227D] font-bold">Team Achieve</h2>
          <p class="text-[#5E5E5E] font-bold">
            Your perfect solution for funding your desires
          </p>
        </div>
      </div>

      <!-- Right Panel -->
      <div
        class="w-full flex flex-col items-center justify-center md:w-1/2 p-8"
      >
        <div class="w-full md:hidden flex items-center justify-center my-4">
          <img :src="Logo" alt="Team Achieve Logo" class="mb-4" />
        </div>
        <h2 class="text-3xl font-bold text-center mb-2 text-[#61227D]">
          Welcome Back
        </h2>
        <p class="text-center text-sm text-[#5E5E5E] mb-6">
          Enter your email address and password to access your account.
        </p>

        <form @submit.prevent="submitForm" class="space-y-5 w-full max-w-md">
          <div>
            <label for="email" class="block text-sm font-medium text-[#3E3E3E]"
              >Email Address *</label
            >
            <input
              v-model="email"
              type="text"
              id="email"
              placeholder="Enter your email"
              class="mt-1 w-full px-4 py-[.7rem] border rounded-md focus:ring-2 focus:ring-purple-500 focus:outline-none"
              :class="{ 'border-red-500': errors.email }"
            />
            <p v-if="errors.email" class="text-red-500 text-sm mt-1">
              {{ errors.email }}
            </p>
          </div>

          <div>
            <label
              for="password"
              class="block text-sm font-medium text-[#3E3E3E]"
              >Password *</label
            >
            <div
              class="mt-1 flex w-full rounded-md border focus-within:ring-2 focus-within:ring-purple-500 overflow-hidden"
              :class="{ 'border-red-500': errors.password }"
            >
              <input
                v-model="password"
                :type="showPassword ? 'text' : 'password'"
                id="password"
                placeholder="Enter your password"
                class="flex-1 px-4 py-[.7rem] focus:outline-none"
              />

              <button
                type="button"
                class="flex text-[#7D7D7D] hover:bg-gray-50 cursor-pointer duration-300 items-center justify-center px-3 bg-gray-100 border-r "
                @click="showPassword = !showPassword"
                tabindex="-1"
              >
                <Eye v-if="showPassword" />
                <EyeOff v-else />
              </button>
            </div>

            <p v-if="errors.password" class="text-red-500 text-sm mt-1">
              {{ errors.password }}
            </p>
          </div>

          <div class="flex items-center justify-between text-sm">
            <label class="flex text-[#7D7D7D] items-center space-x-2">
              <input v-model="rememberMe" type="checkbox" class="rounded" />
              <span>Remember me</span>
            </label>
            <a href="#" class="text-[#61227D] hover:underline"
              >Forgot Password?</a
            >
          </div>

          <button
            type="submit"
            class="w-full bg-purple-700 hover:bg-purple-800 text-white py-2 rounded-md transition duration-300"
          >
            Sign In
          </button>

          <p class="text-center text-sm text-[#5E5E5E]">
            Don’t have an account?
            <a href="#" class="text-purple-600 hover:underline">Sign up</a>
          </p>
        </form>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import { z } from "zod";
import BackgoundImage from "@/assets/image 31.svg";
import Logo from "@/assets/logo.svg";
import { watch } from "vue";
import { Eye, EyeOff } from "lucide-vue-next";

const showPassword = ref(false);

// Form fields
const email = ref("");
const password = ref("");
const rememberMe = ref(false);

// Error state
const errors = ref<Record<string, string>>({});

// Define Zod schema
const loginSchema = z.object({
  email: z.string().min(1, "Email is required").email("Invalid email format"),
  password: z.string().min(6, "Password must be at least 6 characters"),
});

// Submit handler
const submitForm = () => {
  errors.value = {};

  const result = loginSchema.safeParse({
    email: email.value,
    password: password.value,
  });
  
  if (!result.success) {
    result.error.errors.forEach((err) => {
      if (err.path.length > 0) {
        errors.value[err.path[0]] = err.message;
      }
    });
  } else {
    console.log("Submitted:", {
      email: email.value,
      password: password.value,
      rememberMe: rememberMe.value,
    });
  }
};

watch(email, () => {
  if (errors.value.email) {
    errors.value.email = "";
  }
});

watch(password, () => {
  if (errors.value.password) {
    errors.value.password = "";
  }
});
</script>
