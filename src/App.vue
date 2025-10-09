<script setup>
  import { ref } from 'vue'
  import Header from './components/Header.vue'
  
  const headerRef = ref(null)
  
  const openCommandPalette = () => {
    headerRef.value?.openCommandPalette()
  }
</script>

<template>
  <Header ref="headerRef" />
  <div class="container mx-auto px-4 py-8 text-left">
    <h1 class="text-3xl font-bold text-left mb-8 text-white">Diego Silva</h1>
    <strong class="mb-4 block">Desenvolvedor Full Stack</strong>
    <p class="text-gray-500 mb-4 text-balance">Mais vale o <b>simples</b> bem feito que um <b>complexo</b> pouco <b>eficiente</b>.</p>
    <button @click="openCommandPalette" class="flex flex-wrap hover:!bg-[#212024] items-center !border-0 py-1 text-white !bg-transparent relative -left-5 px-5">
      Comece <span class="px-1 text-[#08070b] bg-[#8f9ba8] mx-1 rounded-[2px] uppercase font-bold">por</span>
      <span class="px-1 uppercase font-bold text-[#08070b] bg-[#8f9ba8] mx-1 rounded-[2px]">aqui</span> → 
    </button>
  </div>
  
</template>
