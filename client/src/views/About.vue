<template>
  <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
    <div class="px-4 py-6 sm:px-0">
      <div class="border-4 border-dashed border-gray-200 rounded-lg p-8">
        <h2 class="text-3xl font-bold text-gray-900 mb-6">About</h2>
        
        <div class="bg-white shadow sm:rounded-lg mb-8">
          <div class="px-4 py-5 sm:p-6">
            <h3 class="text-lg leading-6 font-medium text-gray-900 mb-4">
              Technology Stack
            </h3>
            <div class="mt-2 max-w-xl text-sm text-gray-500">
              <ul class="list-disc pl-5 space-y-2">
                <li>Vue 3 with Composition API</li>
                <li>Vue Router for navigation</li>
                <li>Tailwind CSS for styling</li>
                <li>Axios for HTTP requests</li>
                <li>Express backend with ES6</li>
                <li>CORS enabled</li>
                <li>JSON encoding</li>
              </ul>
            </div>
          </div>
        </div>

        <div class="bg-white shadow sm:rounded-lg">
          <div class="px-4 py-5 sm:p-6">
            <h3 class="text-lg leading-6 font-medium text-gray-900 mb-4">
              Server Status
            </h3>
            <button 
              @click="checkHealth"
              class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded mb-4"
              :disabled="checking"
            >
              {{ checking ? 'Checking...' : 'Check Server Health' }}
            </button>
            
            <div v-if="healthStatus" class="mt-4">
              <div class="bg-green-50 border-l-4 border-green-400 p-4">
                <div class="flex">
                  <div class="flex-shrink-0">
                    <svg class="h-5 w-5 text-green-400" viewBox="0 0 20 20" fill="currentColor">
                      <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                    </svg>
                  </div>
                  <div class="ml-3">
                    <p class="text-sm text-green-700">
                      {{ healthStatus.message }}
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

// Composition API reactive state
const healthStatus = ref(null)
const checking = ref(false)

// Composition API method
const checkHealth = async () => {
  checking.value = true
  
  try {
    const response = await axios.get('/api/health')
    healthStatus.value = response.data
  } catch (err) {
    healthStatus.value = { status: 'error', message: err.message }
  } finally {
    checking.value = false
  }
}
</script>
