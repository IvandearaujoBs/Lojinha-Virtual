Opera Doceria — Documentação do Projeto
📌 1. Visão Geral

O Opera Doceria é um projeto pessoal desenvolvido para praticar e demonstrar o uso de Vue.js 3, TailwindCSS, HTML, CSS, e integrações externas como WhatsApp e uma futura API de controle de tempo de pedidos.

A aplicação simula uma loja de doces online, apresentando:

Carrossel de destaques

Catálogo de produtos

Carrinho simples

Página de encomendas

Seleção de quantidade e data

Integração com WhatsApp

Planejamento de API (tempo de produção e entrega)

🎯 2. Objetivos do Projeto
Objetivo Principal

Criar uma interface moderna e responsiva utilizando Vue + Tailwind, explorando elementos reais de um sistema de e-commerce.

Objetivos Secundários

Praticar reatividade e componentização no Vue

Criar layouts elegantes com Tailwind

Trabalhar com listas dinâmicas (v-for)

Simular fluxo real de encomendas

Explorar integrações externas (WhatsApp API)

Preparar estrutura para API de monitoramento de pedidos

🏗 3. Tecnologias Utilizadas
Tecnologia	Utilização
Vue.js 3	Estrutura e lógica da aplicação
<script setup>	Reatividade e sintaxe simplificada
TailwindCSS	Estilização rápida, responsiva e modular
HeroIcons	Ícones leves e compatíveis com Vue
HTML5 / CSS3	Base estrutural e ajustes pontuais
JavaScript (ES6+)	Lógica de interface e manipulação
WhatsApp API (Link)	Envio de pedidos
API futura	Controle de tempo pedido → entrega
🧩 4. Estrutura do Projeto
OPERA DOCERIA
├── .lh
├── .vscode
├── dist
├── node_modules
├── public
│   └── vite.svg
├── src
│   ├── assets
│   │   └── vue.svg
│   ├── components
│   │   └── HelloWorld.vue
│   ├── App.vue
│   ├── main.js
│   └── style.css
├── index.html
├── package-lock.json
├── package.json
├── vite.config.js
└── README.md

🍰 5. Funcionalidades Implementadas
5.1 Header Interativo

O header possui:

Logo do projeto

Acesso à Home

Botão do carrinho com contador

Menu hamburguer

Aba Encomendas

5.2 Carrossel Horizontal de Destaques

Rolagem horizontal suave

Cards coloridos

Totalmente responsivo

Criado usando flex + overflow-x

5.3 Catálogo de Produtos

Seção Delícias da Semana:

Grid responsivo (2 a 5 colunas)

Cards com:

Foto

Nome

Preço

Efeito hover e shadow

Dados armazenados em array reativo

🛒 6. Sistema de Encomendas (Página Encomendas.vue)

A aba Encomendas permite:

Funcionalidades:

Escolher produto

Selecionar quantidade

Selecionar data da entrega

Botão "Enviar Pedido"

Mensagem gerada automaticamente:
Olá! Quero fazer uma encomenda:

Produto: ______
Quantidade: ____
Data da entrega: __/__/____


Ao enviar, o WhatsApp abre com a mensagem pronta.

📲 7. Integração com WhatsApp

Feita através do formato:

https://wa.me/SEU_NUMERO?text=MENSAGEM_CODIFICADA


A mensagem é montada dinamicamente conforme:

Produto selecionado

Quantidade

Data

Observações (opcional, futuro)

⏱ 8. API de Controle de Tempo (Planejada)

A API acompanhará:

Pedido recebido

Pedido aceito

Início da produção

Pronto para retirada / enviado

Pedido finalizado

Rotas planejadas:
POST /pedido
GET /pedido/{id}
PUT /pedido/{id}/producao
PUT /pedido/{id}/entrega
PUT /pedido/{id}/finalizado

Possibilidades:

Tempo estimado de preparo

Tempo estimado de entrega

Linha do tempo visual do pedido

🧪 9. Testes e Recursos Explorados

Este projeto serviu para praticar:

Reatividade com ref()

Loops com v-for

Estruturação de componentes

Carrossel manual

Tailwind aplicado em:

grid

sombras

bordas

animações

Responsividade nativa

Integração com APIs externas

Simulação de carrinho

Geração automática de mensagem

🚀 10. Expansões Planejadas

Carrinho completo (somatório e checkout)

Página de pagamento

Login de usuários

Histórico de compras

Dashboard administrativo

Integração com mapa de entrega

API completa com banco de dados

Cupons e sistema de fidelidade

🏁 11. Conclusão

O Opera Doceria demonstra:

domínio prático de Vue.js 3

experiência com TailwindCSS

capacidade de construir interfaces modernas

exploração de carrosséis, grids e listas dinâmicas

integração real com serviços externos

base sólida para evolução em um e-commerce completo

O projeto continua em expansão e serve como laboratório para novos testes e funcionalidades.



# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

Learn more about IDE Support for Vue in the [Vue Docs Scaling up Guide](https://vuejs.org/guide/scaling-up/tooling.html#ide-support).
