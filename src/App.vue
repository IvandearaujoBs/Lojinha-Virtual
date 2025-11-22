<script setup>
import { ref } from 'vue';
import { HomeIcon } from "@heroicons/vue/24/solid";
import { ShoppingCartIcon, Bars3Icon } from "@heroicons/vue/24/outline"; 



const titulo = "Opera Doceria" 

const activeCard = ref(0); 
const carouselCards = [
  { id: 1, text: "Card de Destaque 1", color: "bg-pink-100" },
  { id: 2, text: "Card de Destaque 2", color: "bg-red-100" },
  { id: 3, text: "Card de Destaque 3", color: "bg-purple-100" },
  { id: 4, text: "Card de Destaque 4", color: "bg-yellow-100" },
  { id: 5, text: "Card de Destaque 5", color: "bg-green-100" },
];

const nextCard = () => {
  if (activeCard.value < carouselCards.length - 1) {
    activeCard.value++;
  } else {
    activeCard.value = 0; 
  }
};

const prevCard = () => {
  if (activeCard.value > 0) {
    activeCard.value--;
  } else {
    activeCard.value = carouselCards.length - 1;
  }
};


const produtos = [
  {
    nome: "Brigadeiro Gourmet",
    preco: "R$ 4,50",
    imagem: "https://saborecia.com.br/wp-content/uploads/2020/08/MG_4421-scaled.jpg",
    alt: "brigadeiro"
  },
  {
    nome: "Donuts de Chocolate",
    preco: "R$ 8,00",
    imagem: "https://www.docemeldoces.com/wp-content/uploads/galeria-doces_22.jpg",
    alt: "donuts"
  },
  {
    nome: "Bolo de Cenoura",
    preco: "R$ 65,00",
    imagem: "https://www.academiaassai.com.br/sites/default/files/styles/noticia_1020x640/public/3_doces_com_baixo_teor_de_acucar_para_oferecer_no_seu_negocio_2.jpg?itok=B90Z5VdI",
    alt: "bolo"
  },
  {
    nome: "Doces Finos",
    preco: "R$ 5,50",
    imagem: "https://casarpontocom-inspiracoes.s3.sa-east-1.amazonaws.com/wp-content/uploads/2023/03/doces-para-casamento-4-Foto-Ananda-Souza-Fotografia-600x400.jpeg",
    alt: "nao conheco"
  },
  {
    nome: "Lasanha de Chocolate",
    preco: "R$ 12,00",
    imagem: "https://guiadacozinha.com.br/wp-content/uploads/2019/10/lasanha-de-chocolate.jpg",
    alt: "bolo-chocolate"
  },
  {
    nome: "Panquecas com Doce de Leite",
    preco: "R$ 15,00",
    imagem: "https://img.cybercook.com.br/imagens/receitas/35/panquecas-de-chocolate-com-doce-de-leite-e-castanhas.jpg",
    alt: "panquecas"
  }
]
</script>

<template>
  <div> 
    <header class="bg-red-100 p-4 shadow-md">
      <div class="max-w-7xl mx-auto flex justify-between items-center">
        <div class="text-2xl font-bold text-pink-700">
          {{ titulo }}
        </div>

        <div class="flex items-center space-x-4">
          
          <button class="relative p-2 rounded-full hover:bg-pink-200 transition">
            <ShoppingCartIcon class="h-6 w-6 text-pink-600" />
            <span class="absolute top-0 right-0 inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-white transform translate-x-1/2 -translate-y-1/2 bg-red-600 rounded-full">3</span>
          </button>

          <button class="relative p-2 rounded-full hover:bg-pink-200 transition"><HomeIcon class="h-6 w-6 text-pink-600" />
            <span class="absolute top-0 right-0 inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-white transform translate-x-1/2 -translate-y-1/2"></span>
          </button>
          
          <button class="p-2 rounded-full hover:bg-pink-200 transition">
            <Bars3Icon class="h-6 w-6 text-pink-600" />
          </button>
        </div>
      </div>
    </header>
    <main class="p-4">
      <!-- Carrossel de Cards com Rolagem Horizontal -->
      <div class="overflow-x-auto whitespace-nowrap py-4 ">
        <div class="inline-flex space-x-4 scroll-auto">
          
          <!-- Card 1 -->
          <div class="w-64 h-48 bg-pink-100 rounded-xl shadow-md flex-shrink-0; flex items-center justify-center">
            <p class="text-pink-700 font-bold text-lg">Card de Destaque 1</p>
          </div>
          
          <!-- Card 2 -->
          <div class="w-64 h-48 bg-pink-100 rounded-xl shadow-md flex-shrink-0; flex items-center justify-center">
            <p class="text-pink-700 font-bold text-lg">Card de Destaque 2</p>
          </div>
          
          <!-- Card 3 -->
          <div class="w-64 h-48 bg-pink-100 rounded-xl shadow-md flex-shrink-0; flex items-center justify-center">
            <p class="text-pink-700 font-bold text-lg">Card de Destaque 3</p>
          </div>

          <!-- Card 4 -->
          <div class="w-64 h-48 bg-pink-100 rounded-xl shadow-md flex-shrink-0; flex items-center justify-center">
            <p class="text-pink-700 font-bold text-lg">Card de Destaque 4</p>
          </div>

          <!-- Card 5 -->
          <div class="w-64 h-48 bg-pink-100 rounded-xl shadow-md flex-shrink-0; flex items-center justify-center">
            <p class="text-pink-700 font-bold text-lg">Card de Destaque 5</p>
          </div>
          
        </div>
      </div>

      <h2 class="text-pink-700 font-bold text-3xl mt-6 mb-4">Delícias da Semana</h2> 
      
      
      <!-- INÍCIO DA LISTA DE CARDS -->
      <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-6"> 
        
        <div v-for="produto in produtos" :key="produto.nome" class="flex flex-col bg-white rounded-xl shadow-lg hover:shadow-xl transition duration-300 overflow-hidden">
          
          <!-- Área da Imagem do Card -->
          <div class="h-48 w-full overflow-hidden">
            <img 
              :src="produto.imagem" 
              :alt="produto.alt"
              class="w-full h-full object-cover transform hover:scale-105 transition duration-500">
          </div>
          
          <!-- Área de Conteúdo do Card (Nome e Preço) -->
          <div class="p-4 `flex-grow">
            <h3 class="text-lg font-semibold text-gray-800 mb-1">{{ produto.nome }}</h3>
            <p class="text-xl font-bold text-pink-600 mt-2">{{ produto.preco }}</p>
          </div>
          
        </div>
        
      </div>
      <!-- FIM DA LISTA DE CARDS -->

    </main>
    
  </div> 
</template>


<style scoped>
.logo {
 height: 6em;
 padding: 1.5em;
 will-change: filter;
 transition: filter 300ms;
}
.logo:hover {
 filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
 filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
