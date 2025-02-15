<template>
  <div class="container d-flex flex-column align-items-center justify-content-center vh-100 bg-gradient">
    <div v-if="stage === 'start'" class="text-center">
      <h1 class="display-4 text-white">Bem-vinda ao Jogo do Desafio!</h1>
      <p class="lead text-white mt-3">Aceite o desafio para descobrir uma surpresa especial!</p>
      <button @click="nextStage" class="btn btn-light btn-lg mt-4">Começar</button>
    </div>

    <div v-else-if="stage === 'level1'" class="text-center">
      <h2 class="h1 text-white">Nível 1: Encontre o Coração</h2>
      <p class="text-white mt-3">Clique no coração para continuar!</p>
      <div class="d-grid gap-2 mt-4" style="grid-template-columns: repeat(3, 1fr);">
        <button 
          v-for="(item, index) in items"
          :key="index"
          @click="itemClicked(index)"
          class="btn btn-lg rounded-circle"
          :class="item ? 'btn-danger' : 'btn-light'"
        >
          <span v-if="item">❤️</span>
        </button>
      </div>
    </div>

    <div v-else-if="stage === 'level2'" class="text-center">
      <h2 class="h1 text-white">Nível 2: Capture as Estrelas</h2>
      <p class="text-white mt-3">Clique em todas as estrelas que aparecem!</p>
      <div class="position-relative mt-4" style="width: 200px; height: 200px; border: 2px solid white; border-radius: 10px;">
        <div 
          v-for="(star, index) in stars"
          :key="index"
          :style="{ top: `${star.y}px`, left: `${star.x}px` }"
          class="position-absolute bg-warning rounded-circle d-flex justify-content-center align-items-center"
          style="width: 40px; height: 40px; cursor: pointer;"
          @click="captureStar(index)"
        >
          ⭐
        </div>
      </div>
    </div>

    <div v-else-if="stage === 'final'" class="text-center">
      <h2 class="h1 text-white">Parabéns, você concluiu o jogo!</h2>
      <p class="text-white mt-3">Agora, o momento especial...</p>
      <button @click="showFinalMessage" class="btn btn-pink btn-lg mt-4">Ver Surpresa</button>
    </div>

    <div v-else-if="stage === 'proposal'" class="text-center">
      <h2 class="h1 text-danger">Quer namorar comigo? ❤️</h2>
      <p class="text-white mt-3">Espero que a resposta seja sim! 😊</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    const createRandomPosition = (max) => Math.random() * max;
    return {
      stage: 'start',
      items: Array.from({ length: 9 }, (_, i) => i === Math.floor(Math.random() * 9)),
      stars: Array.from({ length: 3 }, () => ({ x: createRandomPosition(160), y: createRandomPosition(160) })),
      capturedStars: 0,
    };
  },
  methods: {
    nextStage() {
      const stages = ['start', 'level1', 'level2', 'final'];
      const currentIndex = stages.indexOf(this.stage);
      this.stage = stages[currentIndex + 1] || 'start';
    },
    itemClicked(index) {
      if (this.items[index]) this.nextStage();
      else alert('Tente novamente!');
    },
    captureStar(index) {
      this.stars.splice(index, 1);
      this.capturedStars++;
      if (this.capturedStars === 3) this.nextStage();
    },
    showFinalMessage() {
      this.stage = 'proposal';
    },
  },
};
</script>

<style>
body {
  margin: 0;
  font-family: 'Arial', sans-serif;
  background: #000 !important;
}
.bg-gradient {
  background: linear-gradient(to bottom right, #6a11cb, #2575fc);
}
.btn-pink {
  background-color: #ff69b4 !important;
  color: white !important;
}
</style>
