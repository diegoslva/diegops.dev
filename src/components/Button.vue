<script setup>
import { computed } from 'vue';
import Icon from './AppIcon.vue';

const props = defineProps({
  variant: {
    type: String,
    default: 'default',
    validator: (value) => [
      'default',
      'destructive',
      'outline',
      'secondary',
      'ghost',
      'link',
      'success',
      'warning',
      'danger'
    ].includes(value)
  },
  size: {
    type: String,
    default: 'default',
    validator: (value) => [
      'default',
      'sm',
      'lg',
      'icon',
      'xs',
      'xl'
    ].includes(value)
  },
  as: {
    type: String,
    default: 'button'
  },
  loading: {
    type: Boolean,
    default: false
  },
  iconName: {
    type: String,
    default: null
  },
  iconPosition: {
    type: String,
    default: 'left',
    validator: (value) => ['left', 'right'].includes(value)
  },
  iconSize: {
    type: Number,
    default: null
  },
  fullWidth: {
    type: Boolean,
    default: false
  },
  disabled: {
    type: Boolean,
    default: false
  },
  class: {
    type: String,
    default: ''
  }
});

// Emit events
const emit = defineEmits(['click']);

// Slots
const slots = defineSlots();

// Icon size mapping based on button size
const iconSizeMap = {
  xs: 12,
  sm: 14,
  default: 16,
  lg: 18,
  xl: 20,
  icon: 16,
};

const calculatedIconSize = computed(() => {
  return props.iconSize || iconSizeMap[props.size] || 16;
});

// Computed classes for the button
const buttonClasses = computed(() => {
  const baseClasses = "inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50";
  
  // Variant classes
  const variantClasses = {
    default: "bg-primary text-primary-foreground hover:bg-primary/90",
    destructive: "bg-destructive text-destructive-foreground hover:bg-destructive/90",
    outline: "border border-input hover:bg-accent hover:text-accent-foreground",
    secondary: "bg-secondary text-secondary-foreground hover:bg-secondary/80",
    ghost: "hover:bg-accent hover:text-accent-foreground",
    link: "text-primary underline-offset-4 hover:underline",
    success: "bg-success text-success-foreground hover:bg-success/90",
    warning: "bg-warning text-warning-foreground hover:bg-warning/90",
    danger: "bg-error text-error-foreground hover:bg-error/90",
  };

  // Size classes
  const sizeClasses = {
    default: "h-10 px-4 py-2",
    sm: "h-9 rounded-md px-3",
    lg: "h-11 rounded-md px-8",
    icon: "h-10 w-10",
    xs: "h-8 rounded-md px-2 text-xs",
    xl: "h-12 rounded-md px-10 text-base",
  };

  return [
    baseClasses,
    variantClasses[props.variant],
    sizeClasses[props.size],
    props.fullWidth ? 'w-full' : '',
    props.class
  ].filter(Boolean).join(' ');
});

const handleClick = (event) => {
  emit('click', event);
};

// Icon class
const iconClass = computed(() => {
  if (!slots.default) return '';
  return props.iconPosition === 'left' ? 'mr-2' : 'ml-2';
});
</script>

<template>
  <component 
    :is="as"
    :class="buttonClasses"
    :disabled="disabled || loading"
    @click="handleClick"
  >
    <!-- Loading spinner -->
    <svg 
      v-if="loading" 
      class="animate-spin -ml-1 mr-2 h-4 w-4" 
      fill="none" 
      viewBox="0 0 24 24"
    >
      <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4" />
      <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z" />
    </svg>
    
    <!-- Left icon -->
    <Icon 
      v-if="iconName && iconPosition === 'left'" 
      :name="iconName"
      :size="calculatedIconSize"
      :class="iconClass"
    />
    
    <!-- Content -->
    <slot></slot>
    
    <!-- Right icon -->
    <Icon 
      v-if="iconName && iconPosition === 'right'" 
      :name="iconName"
      :size="calculatedIconSize"
      :class="iconClass"
    />
  </component>
</template>