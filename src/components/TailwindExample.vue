<template>
  <div class="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-lg flex items-center space-x-4 dark:bg-gray-800">
    <div class="shrink-0">
      <div class="h-12 w-12 bg-blue-500 rounded-full flex items-center justify-center">
        <span class="text-white text-lg font-bold">T</span>
      </div>
    </div>
    <div>
      <div class="text-xl font-medium text-black dark:text-white">Tailwind CSS</div>
      <p class="text-gray-500 dark:text-gray-400">Exemplo de uso do Tailwind</p>
    </div>
  </div>
  
  <div class="mt-8 grid grid-cols-1 md:grid-cols-3 gap-4">
    <div class="bg-blue-500 text-white p-4 rounded-lg shadow">
      Card 1
    </div>
    <div class="bg-green-500 text-white p-4 rounded-lg shadow">
      Card 2
    </div>
    <div class="bg-purple-500 text-white p-4 rounded-lg shadow">
      Card 3
    </div>
  </div>
  
  <button class="mt-8 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
    Botão Tailwind
  </button>
</template>

<script setup>
// Sem código adicional necessário
</script>