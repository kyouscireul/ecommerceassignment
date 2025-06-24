<template>
  <teleport to="body">
    <transition name="modal">
      <div v-if="show" class="fixed inset-0 z-[100] flex items-center justify-center p-4">
        <!-- Backdrop with blur -->
        <div 
          class="absolute inset-0 bg-black/50 backdrop-blur-sm"
          @click="emit('close')"
        ></div>

        <!-- Modal container -->
        <div class="relative w-full max-w-md bg-white rounded-xl shadow-2xl overflow-hidden">
          <!-- Close button -->
          <button
            @click="emit('close')"
            class="absolute top-4 right-4 p-1 rounded-full hover:bg-gray-100 transition"
            aria-label="Close login modal"
          >
            <svg class="w-6 h-6 text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
            </svg>
          </button>

          <!-- Modal content -->
          <div class="p-8">
            <div class="text-center mb-8">
              <h2 class="text-3xl font-bold text-gray-900 mb-2">Welcome back</h2>
              <p class="text-gray-600">Sign in to your account</p>
            </div>

            <form @submit.prevent="handleSubmit" class="space-y-5">
              <!-- Email -->
              <div>
                <label for="email" class="block text-sm font-medium text-gray-700 mb-1">Email address</label>
                <input
                  v-model="form.email"
                  type="email"
                  id="email"
                  required
                  class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-600 focus:border-transparent"
                  placeholder="your@email.com"
                >
              </div>

              <!-- Password -->
              <div>
                <div class="flex justify-between items-center mb-1">
                  <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
                  <a href="#" class="text-sm text-purple-600 hover:text-purple-700">Forgot password?</a>
                </div>
                <input
                  v-model="form.password"
                  type="password"
                  id="password"
                  required
                  class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-600 focus:border-transparent"
                  placeholder="••••••••"
                >
              </div>

              <!-- Remember me -->
              <div class="flex items-center">
                <input
                  v-model="form.remember"
                  type="checkbox"
                  id="remember"
                  class="h-4 w-4 text-purple-600 focus:ring-purple-500 border-gray-300 rounded"
                >
                <label for="remember" class="ml-2 block text-sm text-gray-700">Remember me</label>
              </div>

              <!-- Submit button -->
              <button
                type="submit"
                class="w-full bg-purple-600 text-white py-3 px-4 rounded-lg hover:bg-purple-700 transition font-medium flex justify-center items-center"
                :disabled="isLoading"
              >
                <span v-if="!isLoading">Sign in</span>
                <span v-else class="flex items-center">
                  <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                  </svg>
                  Signing in...
                </span>
              </button>

              <!-- Error message -->
              <div v-if="error" class="text-red-500 text-sm text-center py-2 px-3 bg-red-50 rounded-lg">
                {{ error }}
              </div>
            </form>

            <div class="mt-6 text-center text-sm text-gray-600">
              Don't have an account? 
              <a 
                href="#" 
                @click.prevent="emit('showSignup')" 
                class="text-purple-600 hover:text-purple-700 font-medium"
              >
                Sign up
              </a>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </teleport>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  show: Boolean
});

const emit = defineEmits(['close', 'success', 'showSignup']);

const form = ref({
  email: '',
  password: '',
  remember: false
});

const isLoading = ref(false);
const error = ref(null);

const handleSubmit = async () => {
  try {
    isLoading.value = true;
    error.value = null;
    
    // Simulate API call (replace with actual auth)
    await new Promise(resolve => setTimeout(resolve, 1500));
    
    // Validate (replace with real validation)
    if (form.value.email && form.value.password) {
      emit('success', {
        email: form.value.email,
        // In real app, you'd get this from auth response
        token: 'simulated-jwt-token'
      });
      emit('close');
    } else {
      throw new Error('Please fill all fields');
    }
  } catch (err) {
    error.value = err.message || 'Login failed. Please try again.';
  } finally {
    isLoading.value = false;
  }
};
</script>

<style scoped>
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}
</style>