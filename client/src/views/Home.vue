<template>
  <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
    <div class="px-4 py-6 sm:px-0">
      <div class="border-4 border-dashed border-gray-200 rounded-lg p-8">
        <h2 class="text-3xl font-bold text-gray-900 mb-6">Home</h2>
        
        <div class="mb-8">
          <button 
            @click="fetchData"
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
            :disabled="loading"
          >
            {{ loading ? 'Loading...' : 'Fetch Data from API' }}
          </button>
        </div>

        <div v-if="error" class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded mb-4">
          Error: {{ error }}
        </div>

        <div v-if="apiData" class="bg-white shadow overflow-hidden sm:rounded-lg">
          <div class="px-4 py-5 sm:px-6">
            <h3 class="text-lg leading-6 font-medium text-gray-900">API Response</h3>
          </div>
          <div class="border-t border-gray-200">
            <ul class="divide-y divide-gray-200">
              <li v-for="item in apiData" :key="item.id" class="px-4 py-4">
                <div class="flex items-center">
                  <div class="flex-1">
                    <p class="text-sm font-medium text-gray-900">{{ item.name }}</p>
                    <p class="text-sm text-gray-500">ID: {{ item.id }}</p>
                  </div>
                </div>
              </li>
            </ul>
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
const apiData = ref(null)
const loading = ref(false)
const error = ref(null)

// Composition API method
const fetchData = async () => {
  loading.value = true
  error.value = null
  
  try {
    const response = await axios.get('/api/data')
    apiData.value = response.data.data
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}
</script>
