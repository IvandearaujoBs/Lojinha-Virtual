<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';
import { HomeIcon } from "@heroicons/vue/24/solid" 
import { ShoppingCartIcon, Bars3Icon, XMarkIcon } from "@heroicons/vue/24/outline" 

const titulo = "Opera Doceria" 
const activeCard = ref(0);
const carrinho = ref([]);
const mostrarCarrinho = ref(false);
const mostrarConfirmacao = ref(false);

// Estados de Autenticação
const clienteAutenticado = ref(false);
const mostrarModalCadastro = ref(false);
const abaCadastro = ref('login'); // 'login' ou 'cadastro'

// Dados do Cadastro
const formCadastro = ref({
  nome: '',
  email: '',
  telefone: '',
  endereco: '',
  senha: '',
  confirmarSenha: ''
});

// Dados do cliente autenticado
const clienteLogado = ref(null);

// Estados de Entrega
const tipoEntrega = ref(null); // 'entrega', 'retirada', 'encomenda'
const taxaEntrega = ref(15.00); // Taxa padrão de entrega
const dataEncomenda = ref('');
const unidadeEncomenda = ref('duzia'); // 'duzia' ou 'cento'
const quantidadeEncomenda = ref(1); // Quantidade de dúzias ou centos
const etapaConfirmacao = ref(1); // 1 = tipo entrega, 2 = dados entrega, 3 = confirmação final

// Carregar dados do localStorage ao montar
onMounted(() => {
  const clienteSalvo = localStorage.getItem('clienteOpera');
  if (clienteSalvo) {
    clienteLogado.value = JSON.parse(clienteSalvo);
    clienteAutenticado.value = true;
  }
});

const carouselCards = [
  { 
    id: 1, 
    text: "Doce Especial 1", 
    image: "https://boaforma.abril.com.br/wp-content/uploads/sites/2/2021/11/receitas-doces-sem-acucar-5.jpg?quality=70&strip=info" 
  },
  { 
    id: 2, 
    text: "Torta de Morango", 
    image: "https://guiadacozinha.com.br/wp-content/uploads/2019/10/crepe-de-morango-11924-e1628001995160.jpg" 
  },
  { 
    id: 3, 
    text: "Bolo de Chocolate", 
    image: "https://cdn.awsli.com.br/1898/1898256/arquivos/torta-de-halloween-doce-de-halloween-doce-torta-de-chocolate-copacabana-leblon-c.jpg" 
  },
  { 
    id: 4, 
    text: "Doces Finos", 
    image: "https://casarpontocom-inspiracoes.s3.sa-east-1.amazonaws.com/wp-content/uploads/2023/03/doces-para-casamento-4-Foto-Ananda-Souza-Fotografia-600x400.jpeg" 
  },
  { 
    id: 5, 
    text: "Lasanha Doce", 
    image: "https://guiadacozinha.com.br/wp-content/uploads/2019/10/lasanha-de-chocolate.jpg" 
  },
];

let carouselInterval = null;

onMounted(() => {
  carouselInterval = setInterval(() => {
    nextCard();
  }, 3500); //segundos
});

onUnmounted(() => {
  if (carouselInterval) clearInterval(carouselInterval);
});

const nextCard = () => {
  activeCard.value = (activeCard.value + 1) % carouselCards.length;
};

const prevCard = () => {
  activeCard.value = (activeCard.value - 1 + carouselCards.length) % carouselCards.length;
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
    nome: "Cheesecake de Morango",
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
  },
  {
    nome: "brownie",
    preco: "R$ 15,00",
    imagem: "https://static.itdg.com.br/images/360-240/5d139b703a9df7604abf863a3bd76aa6/252863-shutterstock-1938293728.jpg",
    alt: "panquecas"
  },
  {
    nome: "Panquecas Americanas",
    preco: "R$ 15,00",
    imagem: "https://www.mococa.com.br/wp-content/uploads/2022/03/Panquecas-americanas.jpeg",
    alt: "panquecas"
  },
  {
    nome: "Cone Trufado",
    preco: "R$ 15,00",
    imagem: "https://tudosobrebrigadeirogourmet.com/wp-content/uploads/2025/04/Cone-Trufado-para-fazer-na-pascoa.webp",
    alt: "panquecas"
  },
  {
    nome: "Copo da Felicidade",
    preco: "R$ 15,00",
    imagem: "https://cdn-productdbimages.barry-callebaut.com/sites/default/files/styles/web_gm_callebaut-detail/public/externals/a75f2115a3abfe68abcbc9e761979dfb.jpg?itok=JltXmnDQ",
    alt: "panquecas"
  },
]

// Funções de Autenticação
const realizarCadastro = () => {
  if (!formCadastro.value.nome || !formCadastro.value.email || !formCadastro.value.telefone) {
    alert('Por favor, preencha todos os campos obrigatórios!');
    return;
  }

  if (formCadastro.value.senha !== formCadastro.value.confirmarSenha) {
    alert('As senhas não coincidem!');
    return;
  }

  if (formCadastro.value.senha.length < 6) {
    alert('A senha deve ter pelo menos 6 caracteres!');
    return;
  }

  // Criar cliente
  clienteLogado.value = {
    nome: formCadastro.value.nome,
    email: formCadastro.value.email,
    telefone: formCadastro.value.telefone,
    endereco: formCadastro.value.endereco,
    dataCadastro: new Date().toLocaleDateString()
  };

  // Salvar no localStorage
  localStorage.setItem('clienteOpera', JSON.stringify(clienteLogado.value));
  
  clienteAutenticado.value = true;
  mostrarModalCadastro.value = false;
  
  // Limpar formulário
  formCadastro.value = {
    nome: '',
    email: '',
    telefone: '',
    endereco: '',
    senha: '',
    confirmarSenha: ''
  };

  alert('Cadastro realizado com sucesso!');
};

const realizarLogin = () => {
  const email = formCadastro.value.email;
  const senha = formCadastro.value.senha;

  if (!email || !senha) {
    alert('Por favor, preencha email e senha!');
    return;
  }

  const clienteSalvo = localStorage.getItem('clienteOpera');
  if (clienteSalvo) {
    const cliente = JSON.parse(clienteSalvo);
    if (cliente.email === email) {
      clienteLogado.value = cliente;
      clienteAutenticado.value = true;
      mostrarModalCadastro.value = false;
      
      formCadastro.value = {
        nome: '',
        email: '',
        telefone: '',
        endereco: '',
        senha: '',
        confirmarSenha: ''
      };

      alert('Login realizado com sucesso!');
    } else {
      alert('Email ou senha incorretos!');
    }
  } else {
    alert('Nenhum cliente cadastrado com esse email. Faça o cadastro primeiro!');
  }
};

const deslogar = () => {
  clienteLogado.value = null;
  clienteAutenticado.value = false;
  carrinho.value = [];
  mostrarCarrinho.value = false;
};

// Funções do carrinho
const adicionarAoCarrinho = (produto) => {
  if (!clienteAutenticado.value) {
    mostrarModalCadastro.value = true;
    alert('Por favor, faça cadastro ou login antes de adicionar produtos ao carrinho!');
    return;
  }

  const itemExistente = carrinho.value.find(item => item.nome === produto.nome);
  
  if (itemExistente) {
    itemExistente.quantidade++;
  } else {
    carrinho.value.push({
      ...produto,
      quantidade: 1,
      precoNumerico: parseFloat(produto.preco.replace('R$ ', '').replace(',', '.'))
    });
  }
};

const removerDoCarrinho = (indice) => {
  carrinho.value.splice(indice, 1);
};

const aumentarQuantidade = (indice) => {
  carrinho.value[indice].quantidade++;
};

const diminuirQuantidade = (indice) => {
  if (carrinho.value[indice].quantidade > 1) {
    carrinho.value[indice].quantidade--;
  } else {
    removerDoCarrinho(indice);
  }
};


const totalCarrinho = computed(() => {
  return carrinho.value.reduce((total, item) => {
    return total + (item.precoNumerico * item.quantidade);
  }, 0).toFixed(2);
});

const totalComTaxa = computed(() => {
  let total = parseFloat(totalCarrinho.value);
  if (tipoEntrega.value === 'entrega') {
    total += parseFloat(taxaEntrega.value);
  }
  return total.toFixed(2);
});

const calcularTotalEncomenda = () => {
  if (tipoEntrega.value !== 'encomenda') return '0.00';
  
  let total = 0;
  carrinho.value.forEach(item => {
    let precoUnitario = item.precoNumerico;
    let unidades = 0;
    
    if (unidadeEncomenda.value === 'duzia') {
      unidades = 12;
    } else if (unidadeEncomenda.value === 'cento') {
      unidades = 100;
    }
    
    total += precoUnitario * unidades * quantidadeEncomenda.value;
  });
  
  return total.toFixed(2);
};

const proximaEtapa = () => {
  if (etapaConfirmacao.value === 1) {
    if (!tipoEntrega.value) {
      alert('Selecione o tipo de entrega!');
      return;
    }
    if (tipoEntrega.value === 'encomenda') {
      if (!dataEncomenda.value) {
        alert('Informe a data da encomenda!');
        return;
      }
      if (quantidadeEncomenda.value < 1) {
        alert('Informe a quantidade de ' + (unidadeEncomenda.value === 'duzia' ? 'dúzias' : 'centos') + '!');
        return;
      }
    }
    etapaConfirmacao.value = 2;
  } else if (etapaConfirmacao.value === 2) {
    etapaConfirmacao.value = 3;
  }
};

const voltarEtapa = () => {
  if (etapaConfirmacao.value > 1) {
    etapaConfirmacao.value--;
  }
};

const confirmarPedido = () => {
  if (carrinho.value.length === 0) {
    alert('Adicione produtos ao carrinho!');
    return;
  }

  let tipoEntregaTexto = '';
  let detalhesEntrega = '';
  let totalFinal = '';
  let itensTexto = '';

  if (tipoEntrega.value === 'entrega') {
    tipoEntregaTexto = '🚗 Entrega em Domicílio';
    detalhesEntrega = `\nEndereço de Entrega: ${clienteLogado.value.endereco}\nTaxa de Entrega: R$ ${parseFloat(taxaEntrega.value).toFixed(2)}`;
    totalFinal = totalComTaxa.value;
    itensTexto = carrinho.value.map(item => `${item.nome} (x${item.quantidade}) - R$ ${(item.precoNumerico * item.quantidade).toFixed(2)}`).join('\n');
  } else if (tipoEntrega.value === 'retirada') {
    tipoEntregaTexto = '🏪 Retirada no Local';
    detalhesEntrega = `\nLocal de Retirada: Opera Doceria`;
    totalFinal = totalCarrinho.value;
    itensTexto = carrinho.value.map(item => `${item.nome} (x${item.quantidade}) - R$ ${(item.precoNumerico * item.quantidade).toFixed(2)}`).join('\n');
  } else if (tipoEntrega.value === 'encomenda') {
    tipoEntregaTexto = `📦 Encomenda (${unidadeEncomenda.value === 'duzia' ? 'Dúzia' : 'Cento'})`;
    const unidadeNome = unidadeEncomenda.value === 'duzia' ? 'Dúzia' : 'Cento';
    const unidadesTotal = unidadeEncomenda.value === 'duzia' ? 12 : 100;
    detalhesEntrega = `\nData da Encomenda: ${new Date(dataEncomenda.value).toLocaleDateString('pt-BR')}\nTipo de Pacote: ${unidadeNome} (${unidadesTotal} unidades)\nQuantidade: ${quantidadeEncomenda.value} ${unidadeEncomenda.value === 'duzia' ? 'dúzia(s)' : 'cento(s)'} (${quantidadeEncomenda.value * unidadesTotal} unidades total)`;
    totalFinal = calcularTotalEncomenda();
    itensTexto = carrinho.value.map(item => {
      const unidadesTotal = unidadeEncomenda.value === 'duzia' ? 12 : 100;
      const precoUnitario = item.precoNumerico;
      const precoTotal = precoUnitario * unidadesTotal * quantidadeEncomenda.value;
      return `${item.nome} - ${quantidadeEncomenda.value}x ${unidadeNome} (${quantidadeEncomenda.value * unidadesTotal} unidades) - R$ ${precoTotal.toFixed(2)}`;
    }).join('\n');
  }

  const mensagem = `*NOVO PEDIDO - OPERA DOCERIA*\n\n📋 *Dados do Cliente:*\nNome: ${clienteLogado.value.nome}\nEmail: ${clienteLogado.value.email}\nTelefone: ${clienteLogado.value.telefone}\n\n${tipoEntregaTexto}${detalhesEntrega}\n\n🛍️ *Itens do Pedido:*\n${itensTexto}\n\n💰 *Total: R$ ${totalFinal}*\n\nData do Pedido: ${new Date().toLocaleDateString('pt-BR')} às ${new Date().toLocaleTimeString('pt-BR')}`;
  
  const numeroWhatsapp = '558695732239'; // Substitua pelo seu número
  const urlWhatsapp = `https://wa.me/${numeroWhatsapp}?text=${encodeURIComponent(mensagem)}`;
  
  window.open(urlWhatsapp, '_blank');
  
  // Limpar carrinho e resetar após enviar
  carrinho.value = [];
  tipoEntrega.value = null;
  dataEncomenda.value = '';
  unidadeEncomenda.value = 'duzia';
  quantidadeEncomenda.value = 1;
  etapaConfirmacao.value = 1;
  mostrarConfirmacao.value = false;
  mostrarCarrinho.value = false;
};
</script>

<template>
  <div> 
    <header class="bg-red-100 p-4 shadow-md">
      <div class="max-w-7xl mx-auto flex justify-between items-center">
        <div class="text-2xl font-bold text-pink-700">
          {{ titulo }}
        </div>

        <div class="flex items-center space-x-4">
          
          <!-- Botão de Login/Logout -->
          <div v-if="!clienteAutenticado" class="flex items-center space-x-2">
            <button 
              @click="mostrarModalCadastro = true; abaCadastro = 'login'"
              class="px-4 py-2 bg-pink-600 hover:bg-pink-700 text-white font-bold rounded-lg transition"
            >
              Entrar
            </button>
          </div>

          <div v-else class="flex items-center space-x-2">
            <span class="text-sm text-pink-700 font-bold">{{ clienteLogado.nome }}</span>
            <button 
              @click="deslogar"
              class="px-3 py-1 bg-red-500 hover:bg-red-600 text-white text-sm font-bold rounded transition"
            >
              Sair
            </button>
          </div>

          <!-- Carrinho -->
          <button @click="mostrarCarrinho = !mostrarCarrinho" class="relative p-2 rounded-full hover:bg-pink-200 transition">
            <ShoppingCartIcon class="h-6 w-6 text-pink-600" />
            <span v-if="carrinho.length > 0" class="absolute top-0 right-0 inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-white transform translate-x-1/2 -translate-y-1/2 bg-red-600 rounded-full">{{ carrinho.length }}</span>
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
      <!-- CARROSSEL AUTOMÁTICO - UMA IMAGEM POR VEZ -->
      <div class="relative max-w-6xl mx-auto h-80 rounded-xl overflow-hidden shadow-lg mb-8">
        <!-- Imagem Ativa -->
        <div class="w-full h-full">
          <img 
            :src="carouselCards[activeCard].image" 
            :alt="carouselCards[activeCard].text"
            class="w-full h-full object-cover transition-opacity duration-500"
          />
          <div class="absolute bottom-0 left-0 right-0 text-pink-500 bg-opacity-50 text-shadow-2xs p-4">
            <h3 class="text-xl font-bold">{{ carouselCards[activeCard].text }}</h3>
          </div>
        </div>

        <!-- Botões de Navegação -->
        <button 
          @click="prevCard"
          class="absolute left-4 top-1/2 transform -translate-y-1/2 bg-white bg-opacity-80 hover:bg-opacity-100 p-3 rounded-full shadow-lg transition-all duration-200 hover:scale-110"
        >
          ‹
        </button>
        <button 
          @click="nextCard"
          class="absolute right-4 top-1/2 transform -translate-y-1/2 bg-white bg-opacity-80 hover:bg-opacity-100 p-3 rounded-full shadow-lg transition-all duration-200 hover:scale-110"
        >
          ›
        </button>

        <!-- Indicadores -->
        <div class="absolute bottom-4 left-1/2 transform -translate-x-1/2 flex space-x-2">
          <button
            v-for="(card, index) in carouselCards"
            :key="card.id"
            @click="activeCard = index"
            class="w-3 h-3 rounded-full transition-all duration-300"
            :class="activeCard === index ? 'bg-white' : 'bg-white bg-opacity-50'"
          />
        </div>
      </div>

      <h2 class="text-pink-700 font-bold text-3xl mt-6 mb-4">Delícias da Semana</h2> 
      
      <!-- INÍCIO DA LISTA DE CARDS - MANTIDO ORIGINAL -->
      <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-6"> 
        
        <div v-for="produto in produtos" :key="produto.nome" class="flex flex-col h-full bg-white rounded-xl shadow-lg hover:shadow-xl transition duration-300 overflow-hidden">
          
          <!-- Área da Imagem do Card -->
          <div class="h-48 w-full overflow-hidden shrink-0">
            <img 
              :src="produto.imagem" 
              :alt="produto.alt"
              class="w-full h-full object-cover transform hover:scale-105 transition duration-500">
          </div>
          
          <!-- Área de Conteúdo do Card (Nome e Preço) -->
          <div class="p-4 flex flex-col grow justify-between">
            <div>
              <h3 class="text-lg font-semibold text-gray-800 mb-1">{{ produto.nome }}</h3>
              <p class="text-xl font-bold text-pink-600 mt-2">{{ produto.preco }}</p>
            </div>
            <button 
              @click="adicionarAoCarrinho(produto)"
              class="mt-4 w-full bg-pink-600 hover:bg-pink-700 text-white font-bold py-2 px-4 rounded-lg transition duration-200"
            >
              Adicionar
            </button>
          </div>
          
        </div>
        
      </div>
      <!-- FIM DA LISTA DE CARDS -->

    </main>

    <!-- DRAWER DO CARRINHO -->
    <div v-if="mostrarCarrinho" class="fixed inset-0 z-40 overflow-hidden">
      <div class="absolute inset-0 bg-scoped bg-opacity-50" @click="mostrarCarrinho = false"></div>
      
      <div class="absolute right-0 top-0 h-full w-96 bg-white shadow-xl overflow-y-auto">
        <!-- Cabeçalho -->
        <div class="sticky top-0 bg-pink-600 text-white p-6 flex justify-between items-center">
          <h2 class="text-2xl font-bold">Carrinho</h2>
          <button @click="mostrarCarrinho = false" class="text-white">
            <XMarkIcon class="h-6 w-6" />
          </button>
        </div>

        <!-- Itens do Carrinho -->
        <div class="p-6">
          <div v-if="carrinho.length === 0" class="text-center text-gray-500 py-8">
            <p>Seu carrinho está vazio</p>
          </div>

          <div v-else>
            <div v-for="(item, index) in carrinho" :key="index" class="mb-4 border-b pb-4">
              <div class="flex justify-between items-start">
                <div>
                  <h3 class="font-semibold text-gray-800">{{ item.nome }}</h3>
                  <p class="text-pink-600 font-bold">R$ {{ (item.precoNumerico * item.quantidade).toFixed(2) }}</p>
                </div>
                <button 
                  @click="removerDoCarrinho(index)"
                  class="text-red-500 hover:text-red-700"
                >
                  ✕
                </button>
              </div>
              
              <div class="flex items-center justify-between mt-2">
                <button 
                  @click="diminuirQuantidade(index)"
                  class="bg-gray-200 hover:bg-gray-300 w-8 h-8 rounded"
                >
                  -
                </button>
                <span class="font-bold">{{ item.quantidade }}</span>
                <button 
                  @click="aumentarQuantidade(index)"
                  class="bg-gray-200 hover:bg-gray-300 w-8 h-8 rounded"
                >
                  +
                </button>
              </div>
            </div>

            <!-- Total -->
            <div class="mt-6 pt-6 border-t-2 border-gray-300">
              <div class="flex justify-between items-center mb-4">
                <span class="font-bold text-lg">Total:</span>
                <span class="font-bold text-2xl text-pink-600">R$ {{ totalCarrinho }}</span>
              </div>

              <button 
                @click="mostrarConfirmacao = true"
                class="w-full bg-pink-600 hover:bg-pink-700 text-white font-bold py-3 px-4 rounded-lg transition duration-200"
              >
                Confirmar Pedido
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- MODAL DE CONFIRMAÇÃO -->
    <div v-if="mostrarConfirmacao" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50">
      <div class="bg-white rounded-lg p-8 max-w-2xl w-full mx-4 max-h-screen overflow-y-auto">
        
        <!-- ETAPA 1: Tipo de Entrega -->
        <div v-if="etapaConfirmacao === 1">
          <h2 class="text-2xl font-bold text-pink-700 mb-6">📦 Tipo de Entrega</h2>
          
          <div class="space-y-3 mb-6">
            <!-- Opção: Entrega em Domicílio -->
            <label class="block p-4 border-2 rounded-lg cursor-pointer transition" 
                   :class="tipoEntrega === 'entrega' ? 'border-pink-600 bg-pink-50' : 'border-gray-300'">
              <input type="radio" v-model="tipoEntrega" value="entrega" class="mr-3" />
              <span class="font-bold text-gray-800">🚗 Entrega em Domicílio</span>
              <p class="text-sm text-gray-600 mt-1">Taxa: R$ {{ parseFloat(taxaEntrega).toFixed(2) }}</p>
              <p class="text-sm text-gray-600">Endereço: {{ clienteLogado.endereco }}</p>
            </label>

            <!-- Opção: Retirada no Local -->
            <label class="block p-4 border-2 rounded-lg cursor-pointer transition" 
                   :class="tipoEntrega === 'retirada' ? 'border-pink-600 bg-pink-50' : 'border-gray-300'">
              <input type="radio" v-model="tipoEntrega" value="retirada" class="mr-3" />
              <span class="font-bold text-gray-800">🏪 Retirada no Local</span>
              <p class="text-sm text-gray-600 mt-1">Sem taxa de entrega</p>
              <p class="text-sm text-gray-600">Opera Doceria</p>
            </label>

            <!-- Opção: Encomenda -->
            <label class="block p-4 border-2 rounded-lg cursor-pointer transition" 
                   :class="tipoEntrega === 'encomenda' ? 'border-pink-600 bg-pink-50' : 'border-gray-300'">
              <input type="radio" v-model="tipoEntrega" value="encomenda" class="mr-3" />
              <span class="font-bold text-gray-800">📦 Encomenda Especial</span>
              <p class="text-sm text-gray-600 mt-1">Pacotes em Dúzia ou Cento</p>
              <p class="text-sm text-gray-600">Com data agendada</p>
            </label>
          </div>

          <!-- Campos para Encomenda -->
          <div v-if="tipoEntrega === 'encomenda'" class="mb-6 p-4 bg-blue-50 rounded-lg border border-blue-200">
            <div class="mb-4">
              <label class="block text-gray-700 font-bold mb-2">📅 Data da Encomenda *</label>
              <input 
                v-model="dataEncomenda"
                type="date" 
                class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-pink-600"
              />
            </div>

            <div>
              <label class="block text-gray-700 font-bold mb-2">📊 Tipo de Pacote *</label>
              <div class="space-y-2">
                <label class="flex items-center p-3 border rounded-lg cursor-pointer" :class="unidadeEncomenda === 'duzia' ? 'border-pink-600 bg-pink-50' : 'border-gray-300'">
                  <input type="radio" v-model="unidadeEncomenda" value="duzia" class="mr-3" />
                  <span class="font-bold">Dúzia (12 unidades)</span>
                </label>
                <label class="flex items-center p-3 border rounded-lg cursor-pointer" :class="unidadeEncomenda === 'cento' ? 'border-pink-600 bg-pink-50' : 'border-gray-300'">
                  <input type="radio" v-model="unidadeEncomenda" value="cento" class="mr-3" />
                  <span class="font-bold">Cento (100 unidades)</span>
                </label>
              </div>
            </div>

            <div class="mt-4">
              <label class="block text-gray-700 font-bold mb-2">📦 Quantidade de {{ unidadeEncomenda === 'duzia' ? 'Dúzias' : 'Centos' }} *</label>
              <div class="flex items-center gap-2">
                <button 
                  @click="quantidadeEncomenda = Math.max(1, quantidadeEncomenda - 1)"
                  class="bg-gray-300 hover:bg-gray-400 w-10 h-10 rounded font-bold"
                >
                  -
                </button>
                <input 
                  v-model.number="quantidadeEncomenda"
                  type="number" 
                  min="1"
                  class="flex-1 px-4 py-2 border border-gray-300 rounded-lg text-center focus:outline-none focus:border-pink-600 font-bold text-lg"
                />
                <button 
                  @click="quantidadeEncomenda++"
                  class="bg-gray-300 hover:bg-gray-400 w-10 h-10 rounded font-bold"
                >
                  +
                </button>
              </div>
              <p class="text-sm text-gray-600 mt-2">
                Total de unidades: {{ quantidadeEncomenda * (unidadeEncomenda === 'duzia' ? 12 : 100) }}
              </p>
            </div>
          </div>

          <!-- Resumo do Pedido -->
          <div class="mb-6 p-4 bg-gray-100 rounded-lg">
            <h3 class="font-bold text-gray-800 mb-2">🛍️ Resumo do Pedido:</h3>
            
            <!-- Resumo Normal (Entrega ou Retirada) -->
            <div v-if="tipoEntrega !== 'encomenda'">
              <div v-for="item in carrinho" :key="item.nome" class="flex justify-between text-sm mb-1">
                <span>{{ item.nome }} x{{ item.quantidade }}</span>
                <span>R$ {{ (item.precoNumerico * item.quantidade).toFixed(2) }}</span>
              </div>
              <div class="border-t border-gray-300 mt-2 pt-2">
                <div class="flex justify-between font-bold text-gray-800">
                  <span>Subtotal:</span>
                  <span>R$ {{ totalCarrinho }}</span>
                </div>
                <div v-if="tipoEntrega === 'entrega'" class="flex justify-between font-bold text-gray-800 mt-1">
                  <span>Taxa de Entrega:</span>
                  <span>R$ {{ parseFloat(taxaEntrega).toFixed(2) }}</span>
                </div>
                <div class="flex justify-between font-bold text-pink-600 text-lg mt-2">
                  <span>Total:</span>
                  <span>R$ {{ totalComTaxa }}</span>
                </div>
              </div>
            </div>

            <!-- Resumo Encomenda -->
            <div v-else>
              <div v-for="item in carrinho" :key="item.nome" class="mb-2 pb-2 border-b border-gray-300">
                <div class="flex justify-between text-sm mb-1">
                  <span class="font-bold">{{ item.nome }}</span>
                  <span class="text-xs bg-pink-200 px-2 py-1 rounded">{{ quantidadeEncomenda }} x {{ unidadeEncomenda === 'duzia' ? 'Dúzia' : 'Cento' }}</span>
                </div>
                <p class="text-xs text-gray-600">
                  {{ quantidadeEncomenda * (unidadeEncomenda === 'duzia' ? 12 : 100) }} unidades
                </p>
                <div class="flex justify-between text-sm font-bold mt-1">
                  <span>Subtotal do item:</span>
                  <span>R$ {{ (item.precoNumerico * (unidadeEncomenda === 'duzia' ? 12 : 100) * quantidadeEncomenda).toFixed(2) }}</span>
                </div>
              </div>
              
              <div class="border-t border-gray-300 mt-2 pt-2">
                <div class="flex justify-between font-bold text-pink-600 text-lg">
                  <span>Total:</span>
                  <span>R$ {{ calcularTotalEncomenda() }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="flex gap-3">
            <button 
              @click="mostrarConfirmacao = false; etapaConfirmacao = 1; tipoEntrega = null"
              class="flex-1 bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-lg transition"
            >
              Cancelar
            </button>
            <button 
              @click="proximaEtapa"
              class="flex-1 bg-pink-600 hover:bg-pink-700 text-white font-bold py-2 px-4 rounded-lg transition"
            >
              Próximo
            </button>
          </div>
        </div>

        <!-- ETAPA 2: Confirmação de Dados -->
        <div v-else-if="etapaConfirmacao === 2">
          <h2 class="text-2xl font-bold text-pink-700 mb-6">✓ Confirmar Dados</h2>

          <!-- Dados do Cliente -->
          <div class="mb-6 p-4 bg-pink-50 rounded-lg border border-pink-200">
            <h3 class="font-bold text-pink-700 mb-2">📋 Dados do Cliente:</h3>
            <p class="text-sm text-gray-700 mb-1"><strong>Nome:</strong> {{ clienteLogado.nome }}</p>
            <p class="text-sm text-gray-700 mb-1"><strong>Email:</strong> {{ clienteLogado.email }}</p>
            <p class="text-sm text-gray-700 mb-1"><strong>Telefone:</strong> {{ clienteLogado.telefone }}</p>
            <p v-if="tipoEntrega === 'entrega'" class="text-sm text-gray-700"><strong>Endereço de Entrega:</strong> {{ clienteLogado.endereco }}</p>
          </div>

          <!-- Dados da Entrega -->
          <div class="mb-6 p-4 bg-blue-50 rounded-lg border border-blue-200">
            <h3 class="font-bold text-blue-700 mb-2">🚚 Dados da Entrega:</h3>
            <p v-if="tipoEntrega === 'entrega'" class="text-sm text-gray-700 mb-1"><strong>Tipo:</strong> 🚗 Entrega em Domicílio</p>
            <p v-else-if="tipoEntrega === 'retirada'" class="text-sm text-gray-700 mb-1"><strong>Tipo:</strong> 🏪 Retirada no Local</p>
            <p v-else class="text-sm text-gray-700 mb-1"><strong>Tipo:</strong> 📦 Encomenda ({{ unidadeEncomenda === 'duzia' ? 'Dúzia' : 'Cento' }})</p>
            
            <p v-if="tipoEntrega === 'entrega'" class="text-sm text-gray-700">
              <strong>Taxa de Entrega:</strong> R$ {{ parseFloat(taxaEntrega).toFixed(2) }}
            </p>
            <div v-if="tipoEntrega === 'encomenda'" class="text-sm text-gray-700">
              <p class="mb-1"><strong>Data:</strong> {{ new Date(dataEncomenda).toLocaleDateString('pt-BR') }}</p>
              <p class="mb-1"><strong>Quantidade:</strong> {{ quantidadeEncomenda }} x {{ unidadeEncomenda === 'duzia' ? 'Dúzia' : 'Cento' }}</p>
              <p><strong>Total de Unidades:</strong> {{ quantidadeEncomenda * (unidadeEncomenda === 'duzia' ? 12 : 100) }}</p>
            </div>
          </div>

          <!-- Resumo Final -->
          <div class="mb-6 p-4 bg-gray-100 rounded-lg">
            <h3 class="font-bold text-gray-800 mb-2">🛍️ Resumo do Pedido:</h3>
            
            <!-- Resumo Normal (Entrega ou Retirada) -->
            <div v-if="tipoEntrega !== 'encomenda'">
              <div v-for="item in carrinho" :key="item.nome" class="flex justify-between text-sm mb-1">
                <span>{{ item.nome }} x{{ item.quantidade }}</span>
                <span>R$ {{ (item.precoNumerico * item.quantidade).toFixed(2) }}</span>
              </div>
              <div class="border-t border-gray-300 mt-2 pt-2">
                <div class="flex justify-between font-bold text-gray-800">
                  <span>Subtotal:</span>
                  <span>R$ {{ totalCarrinho }}</span>
                </div>
                <div v-if="tipoEntrega === 'entrega'" class="flex justify-between font-bold text-gray-800 mt-1">
                  <span>Taxa de Entrega:</span>
                  <span>R$ {{ parseFloat(taxaEntrega).toFixed(2) }}</span>
                </div>
                <div class="flex justify-between font-bold text-pink-600 text-lg mt-2">
                  <span>TOTAL:</span>
                  <span>R$ {{ totalComTaxa }}</span>
                </div>
              </div>
            </div>

            <!-- Resumo Encomenda -->
            <div v-else>
              <div v-for="item in carrinho" :key="item.nome" class="mb-2 pb-2 border-b border-gray-300">
                <div class="flex justify-between text-sm mb-1">
                  <span class="font-bold">{{ item.nome }}</span>
                  <span class="text-xs bg-pink-200 px-2 py-1 rounded">{{ quantidadeEncomenda }} x {{ unidadeEncomenda === 'duzia' ? 'Dúzia' : 'Cento' }}</span>
                </div>
                <p class="text-xs text-gray-600">
                  {{ quantidadeEncomenda * (unidadeEncomenda === 'duzia' ? 12 : 100) }} unidades
                </p>
                <div class="flex justify-between text-sm font-bold mt-1">
                  <span>Subtotal:</span>
                  <span>R$ {{ (item.precoNumerico * (unidadeEncomenda === 'duzia' ? 12 : 100) * quantidadeEncomenda).toFixed(2) }}</span>
                </div>
              </div>
              
              <div class="border-t border-gray-300 mt-2 pt-2">
                <div class="flex justify-between font-bold text-pink-600 text-lg">
                  <span>TOTAL:</span>
                  <span>R$ {{ calcularTotalEncomenda() }}</span>
                </div>
              </div>
            </div>
          </div>

          <div class="flex gap-3">
            <button 
              @click="voltarEtapa"
              class="flex-1 bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-lg transition"
            >
              Voltar
            </button>
            <button 
              @click="confirmarPedido"
              class="flex-1 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded-lg transition"
            >
              ✓ Enviar via WhatsApp
            </button>
          </div>
        </div>

      </div>
    </div>

    <!-- MODAL DE CADASTRO/LOGIN -->
    <div v-if="mostrarModalCadastro" class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50">
      <div class="bg-white rounded-lg p-8 max-w-md w-full mx-4 max-h-screen overflow-y-auto">
        <div class="flex justify-between items-center mb-6">
          <h2 class="text-2xl font-bold text-pink-700">
            {{ abaCadastro === 'login' ? 'Entrar' : 'Cadastro' }}
          </h2>
          <button @click="mostrarModalCadastro = false" class="text-gray-500 hover:text-gray-700">
            <XMarkIcon class="h-6 w-6" />
          </button>
        </div>

        <!-- Abas -->
        <div class="flex gap-2 mb-6 border-b border-gray-300">
          <button
            @click="abaCadastro = 'login'"
            :class="[
              'px-4 py-2 font-bold',
              abaCadastro === 'login' 
                ? 'text-pink-600 border-b-2 border-pink-600' 
                : 'text-gray-500'
            ]"
          >
            Login
          </button>
          <button
            @click="abaCadastro = 'cadastro'"
            :class="[
              'px-4 py-2 font-bold',
              abaCadastro === 'cadastro' 
                ? 'text-pink-600 border-b-2 border-pink-600' 
                : 'text-gray-500'
            ]"
          >
            Cadastro
          </button>
        </div>

        <!-- LOGIN -->
        <div v-if="abaCadastro === 'login'">
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Email</label>
            <input 
              v-model="formCadastro.email"
              type="email" 
              placeholder="seu@email.com"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-pink-600"
            />
          </div>

          <div class="mb-6">
            <label class="block text-gray-700 font-bold mb-2">Senha</label>
            <input 
              v-model="formCadastro.senha"
              type="password" 
              placeholder="••••••"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-pink-600"
            />
          </div>

          <button 
            @click="realizarLogin"
            class="w-full bg-pink-600 hover:bg-pink-700 text-white font-bold py-2 px-4 rounded-lg transition duration-200 mb-4"
          >
            Entrar
          </button>

          <p class="text-center text-gray-600 text-sm">
            Não tem conta? 
            <button 
              @click="abaCadastro = 'cadastro'"
              class="text-pink-600 font-bold hover:underline"
            >
              Cadastre-se
            </button>
          </p>
        </div>

        <!-- CADASTRO -->
        <div v-else>
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Nome Completo *</label>
            <input 
              v-model="formCadastro.nome"
              type="text" 
              placeholder="Seu nome completo"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-pink-600"
            />
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Email *</label>
            <input 
              v-model="formCadastro.email"
              type="email" 
              placeholder="seu@email.com"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-pink-600"
            />
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Telefone *</label>
            <input 
              v-model="formCadastro.telefone"
              type="tel" 
              placeholder="(85) 98765-4321"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-pink-600"
            />
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Endereço</label>
            <input 
              v-model="formCadastro.endereco"
              type="text" 
              placeholder="Seu endereço"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-pink-600"
            />
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Senha *</label>
            <input 
              v-model="formCadastro.senha"
              type="password" 
              placeholder="••••••"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-pink-600"
            />
          </div>

          <div class="mb-6">
            <label class="block text-gray-700 font-bold mb-2">Confirmar Senha *</label>
            <input 
              v-model="formCadastro.confirmarSenha"
              type="password" 
              placeholder="••••••"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-pink-600"
            />
          </div>

          <button 
            @click="realizarCadastro"
            class="w-full bg-pink-600 hover:bg-pink-700 text-white font-bold py-2 px-4 rounded-lg transition duration-200 mb-4"
          >
            Cadastrar
          </button>

          <p class="text-center text-gray-600 text-sm">
            Já tem conta? 
            <button 
              @click="abaCadastro = 'login'"
              class="text-pink-600 font-bold hover:underline"
            >
              Faça login
            </button>
          </p>
        </div>
      </div>
    </div>
    
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