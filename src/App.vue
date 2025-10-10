<script setup>
  import { ref, computed } from 'vue'
  import Header from './components/Header.vue'
  import About from './views/About.vue'
  import Icon from './components/AppIcon.vue'
  
  const headerRef = ref(null)
  const currentPath = ref(window.location.pathname)
  
  const currentComponent = computed(() => {
    if (currentPath.value === '/about') {
      return About
    }
    return null
  })
  
  const openCommandPalette = () => {
    headerRef.value?.openCommandPalette()
  }
  
  // Listen for navigation changes
  const updatePath = () => {
    currentPath.value = window.location.pathname
  }
  
  window.addEventListener('popstate', updatePath)
  
  // Update path on initial load
  updatePath()
</script>

<template>
  <!-- Development Banner -->
  <div class="bg-gray-900 text-gray-400 text-center py-1 text-xs w-fit px-2 rounded-xl">
    Passando por uma reformulação, pode conter erros e falta de Conteúdos.
  </div>
  
  <Header ref="headerRef" />
  
  <!-- About Page -->
  <component v-if="currentComponent" :is="currentComponent" />
  
  <!-- Home Page -->
  <div v-else class="container mx-auto px-4 py-8 text-left">
    <h1 class="text-3xl font-bold text-left mb-8 text-white">Diego Silva</h1>
    <strong class="mb-4 block">Desenvolvedor Full Stack</strong>
    <p class="text-gray-500 mb-4 text-balance">Mais vale o <b>simples</b> bem feito que um <b>complexo</b> pouco <b>eficiente</b>.</p>
    <button @click="openCommandPalette" class="flex flex-wrap hover:!bg-[#212024] items-center !border-0 py-2 text-white !bg-transparent relative px-5">
      Comece <span class="px-1 text-[#08070b] bg-[#8f9ba8] mx-1 rounded-[2px] uppercase font-bold">por</span>
      <span class="px-1 uppercase font-bold text-[#08070b] bg-[#8f9ba8] mx-1 rounded-[2px]">aqui</span> → 
    </button>
  </div>
  
  <!-- Footer -->
  <footer class="mt-16 py-8">
    <div class="container mx-auto px-4 text-center">
      <div class="flex justify-center space-x-6">
        <a href="https://www.linkedin.com/in/diegoslva/" target="_blank" class="text-gray-400 hover:text-white transition-colors flex items-center gap-2">
          <Icon name="Linkedin" :size="20" />
          LinkedIn
        </a>
        <a href="https://github.com/diegoslva" target="_blank" class="text-gray-400 hover:text-white transition-colors flex items-center gap-2">
          <Icon name="Github" :size="20" />
          GitHub
        </a>
      </div>
    </div>
  </footer>
</template>
