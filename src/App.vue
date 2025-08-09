<script setup lang="ts">
import { ref } from 'vue'
import ServiceFrequency from './components/ServiceFrequency.vue'

const scheduleConfig = ref<string>('')

const handleConfigUpdate = (newConfig: string) => {
  scheduleConfig.value = newConfig
  console.log('Schedule config updated:', newConfig)
}
</script>

<template>
  <div class="min-h-screen bg-background p-8">
    <div class="container mx-auto max-w-4xl">
      <h1 class="text-3xl font-bold text-center mb-8 text-foreground">Portable Toilet Service Scheduler</h1>
      
      <ServiceFrequency 
        :schedule-config="scheduleConfig"
        @update:schedule-config="handleConfigUpdate"
      />
    </div>
  </div>
</template>
