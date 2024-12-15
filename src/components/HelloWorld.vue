<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient'

defineProps<{ msg: string }>()

const count = ref(0)
const error = ref<string | null>(null)
const loading = ref(false)

// Example function to test Supabase connection
async function testConnection() {
  try {
    loading.value = true
    const { data, error: err } = await supabase.from('test').select('*').limit(1)
    if (err) throw err
    console.log('Supabase connection successful:', data)
  } catch (e) {
    error.value = e instanceof Error ? e.message : 'An error occurred'
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <button type="button" @click="testConnection" :disabled="loading">
      {{ loading ? 'Testing...' : 'Test Supabase Connection' }}
    </button>
    <p v-if="error" class="error">{{ error }}</p>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Learn more about IDE Support for Vue in the
    <a
      href="https://vuejs.org/guide/scaling-up/tooling.html#ide-support"
      target="_blank"
      >Vue Docs Scaling up Guide</a
    >.
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}

.error {
  color: #ff4444;
  margin-top: 1em;
}

button {
  margin: 0.5em;
}
</style>