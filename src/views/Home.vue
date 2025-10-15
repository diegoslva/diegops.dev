<script setup>
  import { inject } from 'vue'
  import Portfolio from '../components/Portfolio.vue';
  const openCommandPalette = inject('openCommandPalette')
</script>

<template>
  <div class="container mx-auto px-4 pt-0 py-8 text-left">
    <div class="text-3xl font-bold text-left mb-3 text-white text-balance">Me recuso a aceitar um futuro em que a <br><span>Web</span> <span>seja</span> <span style="color: hsl(15 80% 50%)">estática</span>, <span style="color: hsl(15 80% 50%)">padrão</span> e <strong style="color: hsl(15 80% 50%)">chata</strong>.
    
    
      <div class="zzz relative inline top-1 -left-2">
        <span class="z" style="color: hsl(15 80% 50%)">z</span>
        <span class="z" style="color: hsl(15 80% 50%)">z</span>
        <span class="z" style="color: hsl(15 80% 50%)">z</span>
      </div>
    </div>
    
    <div class="flex flex-wrap items-center gap-x-2">
      <!-- <img
        src="/profile.png"
        alt="Diego Silva"
        class="w-10 h-10 rounded-full object-cover border-2 border-white"
      /> -->
      <h1 class="block text-gray-400 font-medium">Diego Silva, Front-end Developer <a href="http://redemagic.com" target="_blank" rel="noopener noreferrer">@redemagic</a> </h1>
    </div>
    

    <!-- <button @click="openCommandPalette" class="flex flex-wrap hover:!bg-[#212024] items-center !border-0 py-2 text-white !bg-transparent relative px-5">
      Comece <span class="px-1 text-[#08070b] bg-[#8f9ba8] mx-1 rounded-[2px] uppercase font-bold">por</span>
      <span class="px-1 uppercase font-bold text-[#08070b] bg-[#8f9ba8] mx-1 rounded-[2px]">aqui</span> → 
    </button> -->

    <Portfolio />

  </div>
</template>

