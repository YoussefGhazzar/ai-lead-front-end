<template>
  <div class="min-h-screen bg-gray-50 dark:bg-gray-900 flex flex-col justify-center py-12 sm:px-6 lg:px-8">
    <div class="sm:mx-auto sm:w-full sm:max-w-md">
      <div class="text-center">
        <h1 class="text-3xl font-bold text-[#0a2342] dark:text-white mb-2">Stitch AI</h1>
        <h2 class="text-xl font-semibold text-gray-900 dark:text-white">Create your account</h2>
        <p class="mt-2 text-sm text-gray-600 dark:text-gray-400">
          Join thousands of users already using Stitch AI.
        </p>
      </div>
    </div>

    <div class="mt-8 mx-4 sm:mx-auto sm:w-full sm:max-w-md">
      <div class="bg-white dark:bg-gray-800 py-8 px-4 shadow-lg rounded-2xl sm:px-10 border border-gray-200 dark:border-gray-700">
        <form @submit.prevent="handleSignUp" class="space-y-6" novalidate>
          <!-- Full Name -->
          <div>
            <label for="fullName" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Full name</label>
            <input
              id="fullName"
              v-model="form.fullName"
              type="text"
              autocomplete="name"
              :class="[
                'block w-full px-3 py-3 border rounded-lg shadow-sm placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-[#0a2342] focus:border-[#0a2342]',
                fullNameError ? 'border-red-300 dark:border-red-600' : 'border-gray-300 dark:border-gray-600',
                'bg-white dark:bg-gray-700 text-gray-900 dark:text-white'
              ]"
              placeholder="Enter your full name"
              @blur="validateFullName"
              @input="fullNameError = ''"
              required
            />
            <p v-if="fullNameError" class="mt-1 text-sm text-red-600 dark:text-red-400">{{ fullNameError }}</p>
          </div>

          <!-- Email -->
          <div>
            <label for="email" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Email address</label>
            <input
              id="email"
              v-model="form.email"
              type="email"
              autocomplete="email"
              :class="[
                'block w-full px-3 py-3 border rounded-lg shadow-sm placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-[#0a2342] focus:border-[#0a2342]',
                emailError ? 'border-red-300 dark:border-red-600' : 'border-gray-300 dark:border-gray-600',
                'bg-white dark:bg-gray-700 text-gray-900 dark:text-white'
              ]"
              placeholder="Enter your email"
              @blur="validateEmail"
              @input="emailError = ''"
              required
            />
            <p v-if="emailError" class="mt-1 text-sm text-red-600 dark:text-red-400">{{ emailError }}</p>
          </div>

          <!-- Password -->
          <div>
            <label for="password" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Password</label>
            <div class="relative">
              <input
                id="password"
                v-model="form.password"
                :type="showPassword ? 'text' : 'password'"
                autocomplete="new-password"
                :class="[
                  'block w-full px-3 py-3 pr-10 border rounded-lg shadow-sm placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-[#0a2342] focus:border-[#0a2342]',
                  passwordError ? 'border-red-300 dark:border-red-600' : 'border-gray-300 dark:border-gray-600',
                  'bg-white dark:bg-gray-700 text-gray-900 dark:text-white'
                ]"
                placeholder="Create a password"
                @blur="validatePassword"
                @input="passwordError = ''"
                required
              />
              <button type="button" @click="showPassword = !showPassword" class="absolute inset-y-0 right-0 pr-3 flex items-center text-gray-400 hover:text-gray-600 dark:hover:text-gray-300" tabindex="-1">
                <svg v-if="!showPassword" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z" />
                </svg>
                <svg v-else class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.875 18.825A10.05 10.05 0 0112 19c-4.478 0-8.268-2.943-9.543-7a9.97 9.97 0 011.563-3.029m5.858.908a3 3 0 114.243 4.243M9.878 9.878l4.242 4.242M9.878 9.878L3 3m6.878 6.878L21 21" />
                </svg>
              </button>
            </div>
            <p v-if="passwordError" class="mt-1 text-sm text-red-600 dark:text-red-400">{{ passwordError }}</p>
            <div v-if="form.password" class="mt-2">
              <div class="flex space-x-1">
                <div v-for="i in 4" :key="i" :class="[ 'h-1 w-full rounded-full', passwordStrength >= i ? getStrengthColor(passwordStrength) : 'bg-gray-200 dark:bg-gray-600' ]"></div>
              </div>
              <p class="mt-1 text-xs text-gray-600 dark:text-gray-400">{{ getStrengthText(passwordStrength) }}</p>
            </div>
          </div>

          <!-- Confirm Password -->
          <div>
            <label for="confirmPassword" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Confirm password</label>
            <input
              id="confirmPassword"
              v-model="form.confirmPassword"
              :type="showConfirmPassword ? 'text' : 'password'"
              :class="[
                'block w-full px-3 py-3 pr-10 border rounded-lg shadow-sm placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-[#0a2342] focus:border-[#0a2342]',
                confirmPasswordError ? 'border-red-300 dark:border-red-600' : 'border-gray-300 dark:border-gray-600',
                'bg-white dark:bg-gray-700 text-gray-900 dark:text-white'
              ]"
              @blur="validateConfirmPassword"
              @input="confirmPasswordError = ''"
              required
            />
            <p v-if="confirmPasswordError" class="mt-1 text-sm text-red-600 dark:text-red-400">{{ confirmPasswordError }}</p>
          </div>

          <!-- Terms -->
          <div class="flex items-start">
            <div class="flex items-center h-5">
              <input id="terms" v-model="form.acceptTerms" type="checkbox" class="h-4 w-4 text-[#0a2342] border-gray-300 dark:border-gray-600 rounded" />
            </div>
            <div class="ml-3 text-sm">
              <label for="terms" class="text-gray-700 dark:text-gray-300">
                I agree to the
                <a href="#" class="font-medium text-[#0a2342] dark:text-blue-400 hover:text-blue-800 dark:hover:text-blue-300">Terms of Service</a>
                and
                <a href="#" class="font-medium text-[#0a2342] dark:text-blue-400 hover:text-blue-800 dark:hover:text-blue-300">Privacy Policy</a>
              </label>
              <p v-if="termsError" class="text-sm text-red-600 dark:text-red-400 mt-1">You must accept the terms</p>
            </div>
          </div>

          <!-- Submit Button -->
          <div>
            <button
              type="submit"
              :disabled="isLoading || !form.acceptTerms"
              class="group relative w-full flex justify-center items-center py-3 px-4 border border-transparent text-sm font-medium rounded-lg text-white bg-[#0a2342] hover:bg-blue-900 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-[#0a2342] disabled:opacity-50 disabled:cursor-not-allowed transition-colors"
            >
              <span v-if="isLoading" class="absolute left-0 inset-y-0 flex items-center pl-3">
                <svg class="animate-spin h-5 w-5 text-white" fill="none" viewBox="0 0 24 24">
                  <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4" />
                  <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z" />
                </svg>
              </span>
              {{ isLoading ? 'Creating account...' : 'Create account' }}
            </button>
          </div>
        </form>

        <div class="mt-6 text-center text-sm text-gray-600 dark:text-gray-400">
          Already have an account?
          <a href="/login" @click="$emit('switch-to-signin')" class="font-medium text-[#0a2342] dark:text-blue-400 hover:text-blue-800 dark:hover:text-blue-300">
            Sign in
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const form = reactive({
  fullName: '',
  email: '',
  password: '',
  confirmPassword: '',
  acceptTerms: false
})

const isLoading = ref(false)
const showPassword = ref(false)
const showConfirmPassword = ref(false)
const fullNameError = ref('')
const emailError = ref('')
const passwordError = ref('')
const confirmPasswordError = ref('')
const termsError = ref('')

const passwordStrength = computed(() => {
  const pwd = form.password
  let strength = 0
  if (pwd.length >= 8) strength++
  if (/[a-z]/.test(pwd) && /[A-Z]/.test(pwd)) strength++
  if (/\d/.test(pwd)) strength++
  if (/[^a-zA-Z\d]/.test(pwd)) strength++
  return strength
})

const getStrengthColor = (strength) => {
  return ['bg-gray-200', 'bg-red-500', 'bg-yellow-500', 'bg-blue-500', 'bg-green-500'][strength] || 'bg-gray-200'
}

const getStrengthText = (strength) => {
  return ['Enter a password', 'Weak password', 'Fair password', 'Good password', 'Strong password'][strength] || ''
}

const validateFullName = () => {
  fullNameError.value = !form.fullName.trim()
    ? 'Full name is required'
    : form.fullName.length < 2
    ? 'Full name must be at least 2 characters'
    : ''
}

const validateEmail = () => {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  emailError.value = !form.email
    ? 'Email is required'
    : !regex.test(form.email)
    ? 'Please enter a valid email address'
    : ''
}

const validatePassword = () => {
  passwordError.value = !form.password
    ? 'Password is required'
    : form.password.length < 8
    ? 'Password must be at least 8 characters'
    : ''
}

const validateConfirmPassword = () => {
  confirmPasswordError.value = !form.confirmPassword
    ? 'Please confirm your password'
    : form.password !== form.confirmPassword
    ? 'Passwords do not match'
    : ''
}

const validateTerms = () => {
  termsError.value = !form.acceptTerms ? 'You must accept the terms' : ''
}

const handleSignUp = async () => {
  validateFullName()
  validateEmail()
  validatePassword()
  validateConfirmPassword()
  validateTerms()

  if (
    fullNameError.value ||
    emailError.value ||
    passwordError.value ||
    confirmPasswordError.value ||
    termsError.value
  ) return

  isLoading.value = true
  try {
    await new Promise((resolve) => setTimeout(resolve, 2000)) // simule l'API
    console.log('User registered:', form)
    router.push('/plans') // ✅ redirection ici
  } catch (error) {
    console.error('Sign up failed', error)
  } finally {
    isLoading.value = false
  }
}

defineEmits(['switch-to-signin'])
onMounted(() => window.scrollTo({ top: 0, behavior: 'smooth' }))
</script>
