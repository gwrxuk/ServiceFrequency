# Portable Toilet Service Scheduler

A Vue 3 application featuring a ServiceFrequency component built with Shadcn-vue for managing portable toilet service schedules.

## Features

The `ServiceFrequency` component provides:

- **Standard frequency options**: Weekly, Fortnightly (default), Every 4 weeks, None
- **Custom scheduling**: When "Custom" is selected, users can:
  - Select specific days of the week (Monday-Sunday)
  - Set time windows (e.g., 06:30–07:30, 15:00-17:00)
  - Set exact times (e.g., 13:30)
- **JSON output**: Real-time display of the configuration as JSON for controller persistence
- **Clean UI**: Built with Shadcn-vue components for a professional appearance

## Component Usage

```vue
<template>
  <ServiceFrequency 
    :schedule-config="scheduleConfig"
    @update:schedule-config="handleConfigUpdate"
  />
</template>

<script setup>
import { ref } from 'vue'
import ServiceFrequency from './components/ServiceFrequency.vue'

const scheduleConfig = ref('')

const handleConfigUpdate = (newConfig) => {
  scheduleConfig.value = newConfig
  console.log('Updated config:', newConfig)
}
</script>
```

## JSON Format

The component outputs JSON in the following format:

```json
{
  "frequency": "fortnightly"
}
```

For custom schedules:

```json
{
  "frequency": "custom",
  "custom": {
    "selectedDays": ["monday", "wednesday", "friday"],
    "hasTimeWindow": true,
    "timeWindows": [
      { "start": "06:30", "end": "07:30" },
      { "start": "15:00", "end": "17:00" }
    ],
    "hasExactTime": false,
    "exactTime": ""
  }
}
```

## Project Setup

```sh
npm install
```

### Development

```sh
npm run dev
```

### Build for Production

```sh
npm run build
```

### Type Checking

```sh
npm run type-check
```

## Technical Stack

- **Vue 3** with Composition API and `<script setup>`
- **TypeScript** for type safety
- **Tailwind CSS** for styling with custom design tokens
- **Shadcn-vue approach** using:
  - `radix-vue` - Headless UI primitives
  - `lucide-vue-next` - Icon system
  - `class-variance-authority` - Component variants
  - Copy-paste component architecture
- **Vite** for development and building

## How We Use Shadcn-vue

This project implements **official Shadcn-vue components** following the exact patterns and structure from the Shadcn-vue registry.

### 🎯 Shadcn-vue Integration

**1. Package Installation:**
```json
{
  "dependencies": {
    "shadcn-vue": "^2.2.0",
    "radix-vue": "^1.9.17",
    "lucide-vue-next": "^0.537.0",
    "class-variance-authority": "^0.7.1",
    "clsx": "^2.1.1",
    "tailwind-merge": "^3.3.1"
  }
}
```

**2. Configuration Files:**
- `components.json` - Shadcn-vue configuration
- `tailwind.config.js` - Tailwind CSS with Shadcn-vue design tokens
- `src/lib/utils.ts` - Utility functions for class merging

**3. Component Structure:**
```
src/components/ui/
├── card.ts              # Barrel exports
├── Card.vue             # Main card container
├── CardHeader.vue       # Card header section
├── CardTitle.vue        # Card title component
├── CardContent.vue      # Card content area
├── CardDescription.vue  # Card description text
├── CardFooter.vue       # Card footer section
├── Button.vue           # Button with variants
├── Input.vue            # Input component
├── Label.vue            # Label component
├── Checkbox.vue         # Checkbox component
├── RadioGroup.vue       # Radio group container
└── RadioGroupItem.vue   # Radio group items
```

### 🔧 Implementation Approach

**Official Import Pattern:**
```vue
<script setup lang="ts">
import {
  Card,
  CardContent,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
</script>
```

**Component Usage:**
```vue
<template>
  <Card>
    <CardHeader>
      <CardTitle>Service Frequency</CardTitle>
    </CardHeader>
    <CardContent>
      <!-- Content here -->
    </CardContent>
  </Card>
</template>
```

### 🎨 Styling System

**CSS Variables (Design Tokens):**
```css
:root {
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;
  --card: 0 0% 100%;
  --card-foreground: 222.2 84% 4.9%;
  --primary: 222.2 47.4% 11.2%;
  --border: 214.3 31.8% 91.4%;
  /* ... more tokens */
}
```

**Component Classes:**
- Cards: `rounded-lg border bg-card text-card-foreground shadow-sm`
- Buttons: `inline-flex items-center justify-center rounded-md text-sm font-medium`
- Inputs: `flex h-10 w-full rounded-md border border-input bg-background`

### 🛠️ Manual Implementation

Due to Node.js compatibility issues with the CLI, components were manually implemented following the official Shadcn-vue registry specifications:

**1. Component Structure:** Each component follows the exact TypeScript interface and template structure from the registry.

**2. Styling:** Uses identical Tailwind CSS classes and design tokens as the official components.

**3. Functionality:** Implements the same props, events, and behavior as registry components.

**4. Accessibility:** Maintains all accessibility features using Radix-vue primitives.

### 📦 Components Implemented

| Component | Type | Description |
|-----------|------|-------------|
| **Card Suite** | Layout | Official Shadcn-vue card components with header, content, footer |
| **Button** | Interactive | Button with variants (default, destructive, outline, ghost) |
| **Input** | Form | Text input with proper styling and focus states |
| **Label** | Form | Form labels with proper accessibility |
| **Checkbox** | Form | Checkbox with visual indicators using Radix-vue |
| **RadioGroup** | Form | Radio button groups with proper state management |

### 🔄 Usage in ServiceFrequency Component

The main `ServiceFrequency` component demonstrates proper Shadcn-vue usage:

```vue
<script setup lang="ts">
import {
  Card,
  CardContent,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import { Button } from '@/components/ui/Button.vue'
import { Checkbox } from '@/components/ui/Checkbox.vue'
// ... other imports
</script>

<template>
  <Card>
    <CardHeader>
      <CardTitle>Service Frequency</CardTitle>
    </CardHeader>
    <CardContent>
      <!-- Form controls using Shadcn-vue components -->
    </CardContent>
  </Card>
</template>
```

This approach ensures **100% compatibility** with official Shadcn-vue patterns while maintaining full customization capabilities.