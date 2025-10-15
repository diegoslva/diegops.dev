<script setup>
import { ref, computed, onMounted, onUnmounted, watch } from 'vue';
import Icon from './AppIcon.vue';
import Button from './Button.vue';

// Fallback para quando o Vue Router não estiver disponível
const route = ref(window.location.pathname);
const router = {
  push: (path) => {
    window.location.href = path;
  }
};

const isCommandOpen = ref(false);
const searchQuery = ref('');
const selectedIndex = ref(0);

const navigationItems = [
  { name: 'Home', path: '/', icon: 'Terminal' },
  { name: 'Sobre', path: '/about', icon: 'User' },
  { name: 'Games', path: '/games', icon: 'Joystick' },
  { name: 'Projetos', path: '/projects', icon: 'Code' },
];

const commandItems = [
  ...navigationItems,
  { name: 'Contato', action: 'contact', icon: 'MessageCircle' },
  { name: 'GitHub', action: 'github', icon: 'Github' },
  { name: 'LinkedIn', action: 'linkedin', icon: 'Linkedin' },
];

const filteredItems = computed(() => 
  commandItems.filter(item =>
    item.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
);

const handleKeyDown = (e) => {
  if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
    e.preventDefault();
    isCommandOpen.value = true;
    searchQuery.value = '';
    selectedIndex.value = 0;
  }

  if (isCommandOpen.value) {
    if (e.key === 'Escape') {
      isCommandOpen.value = false;
    } else if (e.key === 'ArrowDown') {
      e.preventDefault();
      selectedIndex.value = (selectedIndex.value + 1) % filteredItems.value.length;
    } else if (e.key === 'ArrowUp') {
      e.preventDefault();
      selectedIndex.value = selectedIndex.value === 0 ? filteredItems.value.length - 1 : selectedIndex.value - 1;
    } else if (e.key === 'Enter') {
      e.preventDefault();
      handleItemSelect(filteredItems.value[selectedIndex.value]);
    }
  }
};

const handleItemSelect = (item) => {
  if (item.path) {
    window.location.href = item.path;
  } else if (item.action) {
    switch (item.action) {
      case 'newsletter':
        break;
      case 'contact':
         window.open('mailto:diegoslva7@gmail.com', '');
      case 'github':
        window.open('https://github.com/diegoslva', '_blank');
        break;
      case 'linkedin':
        window.open('https://www.linkedin.com/in/diegoslva/', '_blank');
        break;
    }
  }
  isCommandOpen.value = false;
};

const isActivePath = (path) => route.value === path;

onMounted(() => {
  document.addEventListener('keydown', handleKeyDown);
});

onUnmounted(() => {
  document.removeEventListener('keydown', handleKeyDown);
});

watch(filteredItems, () => {
  selectedIndex.value = 0;
});

const openCommandPalette = () => {
  isCommandOpen.value = true;
  searchQuery.value = '';
  selectedIndex.value = 0;
};

defineExpose({
  openCommandPalette
});
</script>

<template>
  <header class="fixed top-0 left-0 right-0 z-50 bg-background/95 backdrop-blur-xl border-b border-border border-b-neutral-700">
    <div class="flex items-center justify-between h-16 px-6">
      <!-- Logo -->
      <div class="flex items-center">
        <a href="/" class="flex items-center space-x-2">
          <div class="w-8 h-8 bg-primary rounded flex items-center justify-center">
            <span class="text-primary-foreground font-bold text-sm">D</span>
          </div>
          <span class="font-semibold text-lg">Diego</span>
        </a>
      </div>

      <!-- Navigation -->
      <nav class="hidden md:flex items-center space-x-8">
        <router-link
          v-for="item in navigationItems"
          :key="item.path"
          :to="item.path"
          :class="`text-sm font-medium transition-colors hover:text-primary ${
            isActivePath(item.path) 
              ? 'text-primary' : 'text-muted-foreground'
          }`"
        >
          {{ item.name }}
        </router-link>
      </nav>

      <!-- Command Palette Trigger & Actions -->
      <div class="flex items-center space-x-4">
        <div class="hidden md:flex">
          <Button
            variant="ghost"
            size="sm"
            @click="isCommandOpen = true"
            class="hidden md:flex items-center space-x-2 text-muted-foreground hover:text-foreground"
          >
            <Icon name="Search" :size="16" />
            <span class="text-xs">Buscar</span>
            <kbd class="text-xs">⌘K</kbd>
          </Button>
        </div>

        <!-- Mobile Menu -->
        <Button
          variant="ghost"
          size="icon"
          class="md:hidden"
          @click="isCommandOpen = true"
        >
          <Icon name="Menu" :size="20" />
        </Button>
      </div>
    </div>
  </header>
  
  <!-- Command Palette -->
  <Teleport to="body">
    <div v-if="isCommandOpen" class="fixed inset-0 z-50 flex items-start justify-center pt-20 px-4">
      <div 
        class="fixed inset-0 bg-black/50 command-backdrop"
        @click="isCommandOpen = false"
      ></div>
      <div class="relative w-full max-w-lg bg-popover rounded-lg shadow-command animate-scale-in bg-[#ffffff14] backdrop-blur-3xl">
        <div class="flex items-center px-4">
          <Icon name="Search" :size="16" class="text-muted-foreground" />
          <input
            type="text"
            placeholder="Type a command or search..."
            v-model="searchQuery"
            class="flex-1 bg-transparent border-0 py-3 px-3 text-sm outline-none placeholder:text-muted-foreground"
            ref="searchInput"
            autofocus
          />
          <kbd class="text-xs text-muted-foreground">ESC</kbd>
        </div>
        <div class="max-h-96 overflow-y-auto py-2">
          <div v-if="filteredItems.length === 0" class="px-4 py-6 text-center text-sm text-muted-foreground">
            No results found.
          </div>
          <template v-else>
            <button
              v-for="(item, index) in filteredItems"
              :key="item.name"
              @click="handleItemSelect(item)"
              :class="`w-full flex items-center space-x-3 px-4 py-2 text-left transition-colors text-xl ${
                index === selectedIndex
                  ? 'bg-accent text-accent-foreground'
                  : 'hover:bg-[#ffffff17]'
              }`"
            >
              <Icon :name="item.icon" :size="22" />
              <span>{{ item.name }}</span>
              <Icon 
                v-if="item.path && isActivePath(item.path)" 
                name="Check" 
                :size="14" 
                class="ml-auto text-primary" 
              />
            </button>
          </template>
        </div>
        <div class="px-4 py-2 text-xs text-muted-foreground">
          Use ↵ to select, ESC to close
        </div>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
.animate-scale-in {
  animation: scale-in 0.15s ease-out forwards;
}

@keyframes scale-in {
  from {
    opacity: 0;
    transform: scale(0.95);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.space-x-2 > * + * {
  margin-left: 0.5rem;
}

.space-x-3 > * + * {
  margin-left: 0.75rem;
}

.space-x-4 > * + * {
  margin-left: 1rem;
}

.space-x-8 > * + * {
  margin-left: 2rem;
}
</style>