<script setup>
import { ref } from 'vue'
import { PAGE_ACTIVITIES, PAGE_PROGRESS, PAGE_TIMELINE, ACTIVITIES_TYPE_LIST } from './constants'
import { normalizePageHash, generateTimelineItems } from './functions'
import TheHeader from './components/TheHeader.vue'
import TheNav from './components/TheNav.vue'
import TheActivities from './pages/TheActivities.vue'
import TheTimeline from './pages/TheTimeline.vue'
import TheProgress from './pages/TheProgress.vue'

const currentPage = ref(normalizePageHash())

const timelineItems = generateTimelineItems()

function goTo(page) {
  currentPage.value = page
}
</script>

<template>
  <TheHeader @navigate="goTo($event)" />
  <main class="flex flex-grow flex-col">
    <TheTimeline v-show="currentPage === PAGE_TIMELINE" :timeline-items="timelineItems" />
    <TheProgress v-show="currentPage === PAGE_PROGRESS" />
    <TheActivities v-show="currentPage === PAGE_ACTIVITIES" :activities="ACTIVITIES_TYPE_LIST" />
  </main>
  <TheNav :current-page="currentPage" @navigate="goTo($event)" />
</template>
