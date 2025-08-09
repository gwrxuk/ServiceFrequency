<template>
  <div class="flex items-center space-x-2">
    <RadioGroupItem
      :id="id"
      :value="value"
      :class="cn(
        'aspect-square h-4 w-4 rounded-full border border-primary text-primary ring-offset-background focus:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50',
        $attrs.class as string
      )"
    >
      <RadioGroupIndicator class="flex items-center justify-center">
        <Circle class="h-2.5 w-2.5 fill-current text-current" />
      </RadioGroupIndicator>
    </RadioGroupItem>
    <Label
      :for="id"
      class="text-sm font-medium leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70"
    >
      <slot />
    </Label>
  </div>
</template>

<script setup lang="ts">
import { RadioGroupItem, RadioGroupIndicator } from 'radix-vue'
import { Circle } from 'lucide-vue-next'
import { cn } from '@/lib/utils'
import Label from './Label.vue'

defineProps<{
  id: string
  value: string
}>()
</script>
