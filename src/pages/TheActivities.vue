<script setup>
import ActivityItem from '../components/ActivityItem.vue'
import { validateActivities } from '../validators'

defineProps({
  activities: {
    required: true,
    type: Array,
    validators: validateActivities
  }
})
</script>
<template>
  <div>
    <ul class="divide-y">
      <ActivityItem
        class="capitalize"
        v-for="activity in activities"
        :key="activity"
        :activity="activity"
      />
    </ul>
  </div>
</template>
