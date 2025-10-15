<script setup>
import { computed } from 'vue'
import Icon from './AppIcon.vue'

// Componente Portfolio com dados internos

// Função para verificar se campanha está ativa
const isCampaignActive = (endDate) => {
  if (!endDate) return true
  return new Date() <= new Date(endDate)
}

// Dados de exemplo para portfolio
const portfolioItems = [
  {
    id: 1,
    title: "Colgate – Seu Sorriso Vale Prêmios",
    description: "Desenvolvimento do todo Front-end para mais uma campanha Rede Magic",
    image: "portfolio/thumb-colgate.webp",
    year: 2019,
    company: "Rede Magic",
    url: "https://www.promocp.com.br",
    fallbackUrl: "https://www.redemagic.com/cases/colgate-seu-sorriso-vale-premios/",
    technologies: ["Laravel"],
    campaignEndDate: "2025-06-08",
  },
  {
    id: 2,
    title: "Itaú – Rock in Rio Forever - 2024",
    description: "Desenvolvimento do todo Front-end e Game para a campanha Rede Magic",
    image: "portfolio/thumb-itau-forever.webp",
    year: 2019,
    company: "Rede Magic",
    url: "https://www.redemagic.com/cases/itau-promocao-rock-in-rio-forever/",
    fallbackUrl: "https://www.redemagic.com/cases/itau-promocao-rock-in-rio-forever/",
    technologies: ["Laravel"],
    campaignEndDate: "2024-06-08"
  },
  {
    id: 3,
    title: "Shineray - Shineray Entrega Tudo",
    description: "Desenvolvimento do todo Front-end e Game para a campanha Rede Magic",
    image: "portfolio/thumb-shineray.webp",
    year: 2019,
    company: "Rede Magic",
    url: "https://www.promocaoshineray.com.br/",
    fallbackUrl: "https://www.redemagic.com/cases/shineray-entrega-tudo/",
    technologies: ["Laravel"],
    campaignEndDate: "2026-02-31"
  },
];

// Função para obter URL correta baseada no status da campanha
const getProjectUrl = (item) => {
  if (item.campaignEndDate) {
    return isCampaignActive(item.campaignEndDate) ? item.url : item.fallbackUrl
  }
  return item.url
}

// Ordenar projetos por ano (mais recentes primeiro)
const sortedPortfolioItems = computed(() => {
  return [...portfolioItems].sort((a, b) => b.id - a.id)
})

defineExpose({
  portfolioItems
});

</script>

<template>
  <div class="mt-10 mb-5 flex flex-wrap items-center justify-between w-full">
    <p class="text-lg">
      Projetos recentes
    </p>
    <a href="#" class="text-[12px]/[100%] uppercase text-white inline-flex items-center cursor-no-drop">Ver todos <span class="ml-1 text-[10px] text-gray-800 px-1 rounded-full bg-gray-400 no-underline relative">em breve</span> <Icon class='ml-1' name="ArrowUpRight" :size="20" /></a>
  </div>
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-2 max-w-[665px] mx-auto">
    <div 
      v-for="item in sortedPortfolioItems" 
      :key="item.id"
      role="group" 
      aria-roledescription="slide" 
      class="min-w-0">
      <a :href="getProjectUrl(item)" target="_blank" rel="noopener noreferrer" class="group h-full block" tabindex="0">
        <div class="border text-card-foreground shadow overflow-hidden border-zinc-200 h-full rounded-md bg-zinc-100/30 transition-colors backdrop-blur-sm dark:border-zinc-800 dark:bg-zinc-900/30">
          <div class="p-0">
            <div class="relative aspect-[16/9] rounded-t-md overflow-hidden">
              <img 
                :alt="item.title" 
                class="object-cover relative transition-transform duration-300 group-hover:scale-105"
                :src="item.image"
                style="position: absolute; height: 100%; width: 100%; inset: 0px; color: transparent;">
            </div>
            <div class="p-2 flex items-start relative">
              <div>
                <h3 class="text-md/[100%] from-black from-30% to-black/50 dark:from-white dark:from-30% dark:to-white/50 bg-clip-text text-balance">
                  {{ item.title }}
                </h3>
                <p class="text-xs text-zinc-200 mb-1">{{ item.company }} • {{ item.year }}</p>
                <p class="text-xs text-zinc-400 text-balance">{{ item.description }}</p>
                <!-- <div v-if="item.campaignEndDate" class="text-[10px] mt-1">
                  <span :class="isCampaignActive(item.campaignEndDate) ? 'text-green-500' : 'text-red-500'">
                    {{ isCampaignActive(item.campaignEndDate) ? '• Campanha Ativa' : '• Campanha Encerrada' }}
                  </span>
                </div> -->
                <!-- <div class="flex flex-wrap gap-1 mt-1">
                  <div 
                    v-for="tech in item.technologies"
                    :key="tech"
                    class="inline-flex items-center rounded-md border font-semibold transition-colors focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2 text-foreground text-[10px] px-1 py-0">
                    {{ tech }}
                  </div>
                </div> -->
              </div>
              <div class="absolute top-2 right-2">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-arrow-up-right size-3 from-black from-30% to-black/50 dark:from-white dark:from-30% dark:to-white/50 bg-clip-text">
                  <path d="M7 7h10v10"></path>
                  <path d="M7 17 17 7"></path>
                </svg>
              </div>
            </div>
          </div>
        </div>
      </a>
    </div>
  </div>
</template>