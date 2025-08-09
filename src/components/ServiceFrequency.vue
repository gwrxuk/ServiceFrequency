<template>
  <div class="w-full max-w-2xl mx-auto space-y-6">
    <!-- Service Frequency Card -->
    <Card>
      <CardHeader>
        <CardTitle>Service Frequency</CardTitle>
      </CardHeader>
      <CardContent class="space-y-4">
        <!-- Frequency Options -->
        <RadioGroup v-model="selectedFrequency" class="space-y-3">
          <RadioGroupItem id="weekly" value="weekly">
            Weekly
          </RadioGroupItem>
          
          <RadioGroupItem id="fortnightly" value="fortnightly">
            Fortnightly (default)
          </RadioGroupItem>
          
          <RadioGroupItem id="monthly" value="monthly">
            Every 4 weeks
          </RadioGroupItem>
          
          <RadioGroupItem id="none" value="none">
            None
          </RadioGroupItem>
          
          <RadioGroupItem id="custom" value="custom">
            Custom
          </RadioGroupItem>
        </RadioGroup>

        <!-- Custom Options Section -->
        <div v-if="selectedFrequency === 'custom'" class="mt-6 p-4 border rounded-lg bg-muted/50 space-y-4">
          <h4 class="font-semibold text-sm text-foreground">Custom Schedule Options</h4>
          
          <!-- Days of Week -->
          <div class="space-y-3">
            <Label class="text-sm font-medium">Days of the week:</Label>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
              <div v-for="day in daysOfWeek" :key="day.value" class="flex items-center space-x-2">
                <Checkbox
                  :id="day.value"
                  :checked="customConfig.selectedDays.includes(day.value)"
                  @update:checked="(checked) => toggleDay(day.value, checked)"
                />
                <Label :for="day.value" class="text-sm">{{ day.label }}</Label>
              </div>
            </div>
          </div>

          <!-- Time Window Option -->
          <div class="space-y-3">
            <div class="flex items-center space-x-2">
              <Checkbox
                id="time-window"
                :checked="customConfig.hasTimeWindow"
                @update:checked="onTimeWindowChange"
              />
              <Label for="time-window" class="text-sm font-medium">Specify time window(s)</Label>
            </div>
            
            <div v-if="customConfig.hasTimeWindow" class="ml-6 space-y-3">
              <div v-for="(window, index) in customConfig.timeWindows" :key="index" class="flex items-center space-x-2">
                <Input
                  v-model="window.start"
                  type="time"
                  placeholder="Start time"
                  class="w-32"
                />
                <span class="text-sm text-muted-foreground">to</span>
                <Input
                  v-model="window.end"
                  type="time"
                  placeholder="End time"
                  class="w-32"
                />
                <Button
                  v-if="customConfig.timeWindows.length > 1"
                  @click="removeTimeWindow(index)"
                  variant="destructive"
                  size="sm"
                >
                  Remove
                </Button>
              </div>
              <Button
                @click="addTimeWindow"
                variant="outline"
                size="sm"
              >
                + Add time window
              </Button>
            </div>
          </div>

          <!-- Exact Time Option -->
          <div class="space-y-3">
            <div class="flex items-center space-x-2">
              <Checkbox
                id="exact-time"
                :checked="customConfig.hasExactTime"
                @update:checked="onExactTimeChange"
              />
              <Label for="exact-time" class="text-sm font-medium">Set exact time</Label>
            </div>
            
            <div v-if="customConfig.hasExactTime" class="ml-6">
              <Input
                v-model="customConfig.exactTime"
                type="time"
                placeholder="Exact time"
                class="w-32"
              />
            </div>
          </div>
        </div>
      </CardContent>
    </Card>

    <!-- JSON Output Display -->
    <Card>
      <CardHeader>
        <CardTitle>JSON Configuration Output</CardTitle>
      </CardHeader>
      <CardContent>
        <div class="bg-muted p-4 rounded-md">
          <pre class="text-sm text-muted-foreground overflow-auto whitespace-pre-wrap">{{ formattedJsonOutput }}</pre>
        </div>
      </CardContent>
    </Card>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch, onMounted } from 'vue'
import {
  Card,
  CardContent,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import Label from './ui/Label.vue'
import Input from './ui/Input.vue'
import RadioGroup from './ui/RadioGroup.vue'
import RadioGroupItem from './ui/RadioGroupItem.vue'
import Checkbox from './ui/Checkbox.vue'
import Button from './ui/Button.vue'

// Props
interface Props {
  scheduleConfig?: string
}

const props = withDefaults(defineProps<Props>(), {
  scheduleConfig: ''
})

// Emits
const emit = defineEmits<{
  'update:scheduleConfig': [value: string]
}>()

// Types
interface TimeWindow {
  start: string
  end: string
}

interface CustomConfig {
  selectedDays: string[]
  hasTimeWindow: boolean
  timeWindows: TimeWindow[]
  hasExactTime: boolean
  exactTime: string
}

interface ScheduleConfig {
  frequency: string
  custom?: CustomConfig
}

// Reactive state
const selectedFrequency = ref<string>('fortnightly')

const daysOfWeek = [
  { value: 'monday', label: 'Monday' },
  { value: 'tuesday', label: 'Tuesday' },
  { value: 'wednesday', label: 'Wednesday' },
  { value: 'thursday', label: 'Thursday' },
  { value: 'friday', label: 'Friday' },
  { value: 'saturday', label: 'Saturday' },
  { value: 'sunday', label: 'Sunday' }
]

const customConfig = ref<CustomConfig>({
  selectedDays: [],
  hasTimeWindow: false,
  timeWindows: [{ start: '', end: '' }],
  hasExactTime: false,
  exactTime: ''
})

// Methods
const toggleDay = (day: string, checked: boolean) => {
  if (checked) {
    if (!customConfig.value.selectedDays.includes(day)) {
      customConfig.value.selectedDays.push(day)
    }
  } else {
    const index = customConfig.value.selectedDays.indexOf(day)
    if (index > -1) {
      customConfig.value.selectedDays.splice(index, 1)
    }
  }
}

const addTimeWindow = () => {
  customConfig.value.timeWindows.push({ start: '', end: '' })
}

const removeTimeWindow = (index: number) => {
  customConfig.value.timeWindows.splice(index, 1)
}

const onTimeWindowChange = (checked: boolean) => {
  customConfig.value.hasTimeWindow = checked
  if (!checked) {
    customConfig.value.timeWindows = [{ start: '', end: '' }]
  }
}

const onExactTimeChange = (checked: boolean) => {
  customConfig.value.hasExactTime = checked
  if (!checked) {
    customConfig.value.exactTime = ''
  }
}

// Computed properties
const currentConfig = computed<ScheduleConfig>(() => {
  const config: ScheduleConfig = {
    frequency: selectedFrequency.value
  }

  if (selectedFrequency.value === 'custom') {
    const customData: CustomConfig = {
      selectedDays: [...customConfig.value.selectedDays],
      hasTimeWindow: customConfig.value.hasTimeWindow,
      timeWindows: customConfig.value.hasTimeWindow 
        ? customConfig.value.timeWindows.filter(w => w.start && w.end)
        : [],
      hasExactTime: customConfig.value.hasExactTime,
      exactTime: customConfig.value.hasExactTime ? customConfig.value.exactTime : ''
    }

    config.custom = customData
  }

  return config
})

const formattedJsonOutput = computed(() => {
  return JSON.stringify(currentConfig.value, null, 2)
})

// Initialize from prop
const initializeFromProp = () => {
  if (props.scheduleConfig) {
    try {
      const config: ScheduleConfig = JSON.parse(props.scheduleConfig)
      selectedFrequency.value = config.frequency || 'fortnightly'
      
      if (config.custom && selectedFrequency.value === 'custom') {
        customConfig.value = {
          selectedDays: config.custom.selectedDays || [],
          hasTimeWindow: config.custom.hasTimeWindow || false,
          timeWindows: config.custom.timeWindows?.length 
            ? config.custom.timeWindows 
            : [{ start: '', end: '' }],
          hasExactTime: config.custom.hasExactTime || false,
          exactTime: config.custom.exactTime || ''
        }
      }
    } catch (error) {
      console.warn('Invalid schedule config JSON, using default')
      selectedFrequency.value = 'fortnightly'
    }
  }
}

// Watch for changes and emit updates
watch(currentConfig, (newConfig) => {
  emit('update:scheduleConfig', JSON.stringify(newConfig))
}, { deep: true })

// Initialize on mount
onMounted(() => {
  initializeFromProp()
})
</script>