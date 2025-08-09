<script setup lang="ts">
import { cn } from '@/lib/utils'

interface Props {
  class?: any
}

defineProps<Props>()
</script>

<template>
  <p :class="cn('text-sm text-muted-foreground', $props.class)">
    <slot />
  </p>
</template>