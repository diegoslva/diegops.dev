<script setup>
import { computed } from 'vue';
import * as LucideIcons from 'lucide-vue-next';
import { HelpCircle } from 'lucide-vue-next';

const props = defineProps({
  name: {
    type: String,
    required: true
  },
  size: {
    type: [Number, String],
    default: 24
  },
  color: {
    type: String,
    default: 'currentColor'
  },
  class: {
    type: String,
    default: ''
  },
  strokeWidth: {
    type: [Number, String],
    default: 2
  }
});

const IconComponent = computed(() => {
  // O nome do ícone deve estar em PascalCase para o lucide-vue-next
  if (!props.name) return HelpCircle;
  return LucideIcons[props.name] || HelpCircle;
});

const isDefault = computed(() => {
  return !LucideIcons[props.name];
});
</script>

<template>
  <component
    :is="IconComponent"
    :size="size"
    :color="isDefault ? 'gray' : color"
    :stroke-width="strokeWidth"
    :class="class"
  />
</template>