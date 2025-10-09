<template>
  <div class="container mx-auto px-4 py-8 pt-24 text-left max-w-3xl">
    <!-- Intro Section -->
    <div class="flex flex-col md:flex-row justify-between mb-12">
      <div class="mb-8 md:mb-0">
        <img
          src="/profile.png"
          alt="Diego"
          class="w-80 h-80 rounded-lg object-cover"
        />
      </div>
      <div class="md:w-1/2 text-left">
        <p class="mt-4 md:-mt-2 mb-4">
          <strong>Olá, eu sou Diego Silva</strong>
          Desenvolvedor Web com mais de 10 anos de experiência, trabalhando com PHP, HTML, CSS e JavaScript.
        </p>
        <p class="mb-4">
          Sou <strong>Desenvolvedor Full Stack</strong> especializado Front end, porém utilizo diversas ferramentas e tecnologias.
          Atualmente moro em <strong>São Paulo, SP</strong>.
        </p>
        <p class="mb-4">
          <strong>Nos ultomos anos</strong>, tenho me aventurado com criação de games em JS e Godot Engine. Já trabalhei com as mais diversas tecnologias e ferramentas, como, PHP(laravel), Vue.js, Next.js, Unity3D, Blender, Figma. Photoshop.
        </p>
      </div>
    </div>

    <!-- Career Section -->
    <h2 class="text-2xl font-bold mb-6">Carreira</h2>
    <div class="space-y-10">
      <div v-for="(item, index) in careerItems" :key="index" class="mb-10">
        <h3 class="text-xl font-semibold mb-2">{{ item.jobTitle }}</h3>
        <p class="mb-1">
          <template v-if="item.companyUrl">
            <a  :href="item.companyUrl" target="_blank" class="text-primary hover:underline font-bold underline">
              {{ item.company }}
            </a>
          </template>

          <template v-else>
            <span class="text-primary font-bold">
              {{ item.company }}
            </span>
          </template>
        

          <span> • {{ item.location }}</span>
        </p>
        <p class="text-muted-foreground">
          <span>{{ formatDate(item.startDate) }}</span>
          <span> – </span>
          <span>{{ item.endDate ? formatDate(item.endDate) : 'Presente' }}</span>
          <span> • </span>
          <span>{{ getDuration(item.startDate, item.endDate) }}</span>
        </p>
      </div>
    </div>

    <!-- Toast -->
    <div v-if="showToast" class="fixed bottom-4 right-4 bg-green-600 text-white px-4 py-2 rounded shadow-lg">
      <div class="font-semibold">{{ toastTitle }}</div>
      <div class="text-sm">{{ toastDescription }}</div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Icon from '../components/AppIcon.vue'

const description = "Diego Silva é um desenvolvedor brasileiro especializado em tecnologias web modernas. Atualmente vive em São Paulo, onde trabalha como desenvolvedor full stack focado em Vue.js, Node.js e PHP. Sua paixão por desenvolvimento de software e compartilhamento de conhecimento o levou a criar diversos projetos open source. Antes de se especializar em desenvolvimento web, Diego explorou diferentes áreas da programação, sempre buscando as melhores práticas e tecnologias mais eficientes."

const showToast = ref(false)
const toastTitle = ref('')
const toastDescription = ref('')

const careerItems = [
  {
    jobTitle: 'Desenvolvedor Front-end',
    company: 'Rede Magic',
    companyUrl: 'https://redemagic.com',
    location: 'São Paulo, SP',
    startDate: '2021-01-01',
    endDate: null
  },
  {
    jobTitle: 'Desenvolvedor Web',
    company: 'Rede Digital',
    companyUrl: 'https://renovedigital.com.br',
    location: 'São Paulo, SP',
    startDate: '2018-01-01',
    endDate: '2021-10-31'
  },
  {
    jobTitle: 'Desenvolvedor Web Freelancer',
    company: 'Freelancer',
    companyUrl: '#',
    location: 'São Paulo, SP',
    startDate: '2018-12-01',
    endDate: '2018-12-31'
  },
  {
    jobTitle: 'Desenvolvedor Front-end',
    company: 'MKT Virtual',
    companyUrl: 'https://mktvirtual.com.br',
    location: 'São Paulo, SP',
    startDate: '2017-01-01',
    endDate: '2017-12-31'
  },
  {
    jobTitle: 'Desenvolvedor Front-End',
    company: 'Plug7 Inteligência Web',
    companyUrl: 'https://plug7.com.br',
    location: 'São Paulo, SP',
    startDate: '2014-01-01',
    endDate: '2016-12-31'
  },
  {
    jobTitle: 'Desenvolvedor Front-end',
    company: 'Pegasus Web',
    companyUrl: '',
    location: 'São Paulo, SP',
    startDate: '2013-01-01',
    endDate: '2014-12-31'
  }
]

const formatDate = (dateString) => {
  const date = new Date(dateString)
  return date.toLocaleDateString('pt-BR', { month: 'short', year: 'numeric' })
}

const getDuration = (startDate, endDate) => {
  const start = new Date(startDate)
  const end = endDate ? new Date(endDate) : new Date()
  
  const diffTime = Math.abs(end - start)
  const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24))
  const years = Math.floor(diffDays / 365)
  const months = Math.floor((diffDays % 365) / 30)
  
  let duration = ''
  if (years > 0) {
    duration += `${years} ano${years > 1 ? 's' : ''} `
  }
  if (months > 0) {
    duration += `${months} ${months === 1 ? 'mês' : 'meses'}`
  }
  
  return duration.trim() || '1 mês'
}

const copyBio = () => {
  navigator.clipboard.writeText(description)
  toastTitle.value = 'Copiado!'
  toastDescription.value = 'Agora você pode colar em qualquer lugar.'
  showToast.value = true
  setTimeout(() => showToast.value = false, 3000)
}

const downloadHeadshot = () => {
  toastTitle.value = 'Baixando...'
  toastDescription.value = 'Você pode usar esta foto em seu site.'
  showToast.value = true
  setTimeout(() => showToast.value = false, 3000)
}
</script>