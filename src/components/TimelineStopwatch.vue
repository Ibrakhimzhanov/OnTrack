<script setup>
import { ArrowPathIcon, PauseIcon, PlayIcon } from '@heroicons/vue/24/outline'
import BaseButton from './BaseButton.vue'
import { BUTTON_TYPE_SUCCESS, MILLISECONDS_IN_SECOND } from '../constants'
import { BUTTON_TYPE_WARNING } from '../constants'
import { BUTTON_TYPE_DANGER } from '../constants'
import { formatSeconds } from '../functions'
import { isTimelineItemValid } from '../validators'
import { inject, ref } from 'vue'

const props = defineProps({
  timelineItem: {
    required: true,
    type: Object,
    validator: isTimelineItemValid
  }
})
const updateTimelineItemActivitySeconds = inject('updateTimelineItemActivitySeconds')

const seconds = ref(props.timelineItem.activitySeconds)
const isRunning = ref(false)

const isStartButtonDisabled = props.timelineItem.hour !== new Date().getHours()

function start() {
  isRunning.value = setInterval(() => {
    updateTimelineItemActivitySeconds(props.timelineItem, 1)
    seconds.value++
  }, MILLISECONDS_IN_SECOND)
}

function stop() {
  clearInterval(isRunning.value)

  isRunning.value = false
}

function reset() {
  stop()

  updateTimelineItemActivitySeconds(props.timelineItem, -seconds.value)
  seconds.value = 0
}
</script>
<template>
  <div class="flex w-full gap-2">
    <BaseButton :type="BUTTON_TYPE_DANGER" :disabled="!seconds" @click="reset"
      ><ArrowPathIcon class="h-6"
    /></BaseButton>
    <div class="flex flex-grow items-center rounded bg-gray-100 px-2 font-mono text-3xl">
      {{ formatSeconds(seconds) }}
    </div>
    <BaseButton v-if="isRunning" :type="BUTTON_TYPE_WARNING" @click="stop"
      ><PauseIcon class="h-6"
    /></BaseButton>
    <BaseButton v-else :type="BUTTON_TYPE_SUCCESS" :disabled="isStartButtonDisabled" @click="start"
      ><PlayIcon class="h-6"
    /></BaseButton>
  </div>
</template>
