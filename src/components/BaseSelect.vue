<script setup>
import BaseButton from './BaseButton.vue'
import { XMarkIcon } from '@heroicons/vue/24/outline'
import { isNumberOrNull, isUndefinedOrNull, validateSelectOptions } from '../validators'
import { computed } from 'vue'

const props = defineProps({
  selected: Number,
  placeholder: {
    required: true,
    type: String
  },
  options: {
    required: true,
    type: Array,
    validator: validateSelectOptions
  }
})

const emit = defineEmits({
  select: isNumberOrNull
})

const isNotSelected = computed(() => isUndefinedOrNull(props.selected))
</script>
<template>
  <div class="flex gap-2">
    <BaseButton @click="emit('select', null)">
      <XMarkIcon class="h-8" />
    </BaseButton>
    <select
      name=""
      id=""
      class="w-full truncate rounded bg-gray-100 px-2 py-1 text-2xl outline-none"
      @change="emit('select', +$event.target.value)"
    >
      <option :selected="isNotSelected" disabled value="">
        {{ placeholder }}
      </option>
      <option
        v-for="{ value, label } in options"
        :key="value"
        :value="value"
        :selected="value === selected"
      >
        {{ label }}
      </option>
    </select>
  </div>
</template>
