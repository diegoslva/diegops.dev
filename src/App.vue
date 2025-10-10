<script setup>
  import { ref, provide } from 'vue'
  import Header from './components/Header.vue'
  import Icon from './components/AppIcon.vue'
  
  const headerRef = ref(null)
  
  const openCommandPalette = () => {
    headerRef.value?.openCommandPalette()
  }
  
  provide('openCommandPalette', openCommandPalette)
</script>

<template>
  <!-- Development Banner -->
  <div class="bg-gray-900 text-gray-400 text-center py-1 text-xs w-fit px-2 rounded-xl">
    Passando por uma reformulação, pode conter erros e falta de Conteúdos.
  </div>
  
  <Header ref="headerRef" />
  
  <router-view />
  
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
