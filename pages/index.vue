<template>
  <v-app>
    <v-container class="text-center">
      <h1 class="mb-5">Jogo da Memória</h1>
      <v-row justify="center" align="center">
        <v-col cols="1" v-for="(card, index) in cards" :key="index">
          <v-card
            class="memory-card"
            :class="{ flipped: card.isFlipped || card.isMatched }"
            @click="flipCard(index)"
            height="300px"
          >
            <v-img
              v-if="card.isFlipped || card.isMatched"
              :src="card.image"
              height="100%"
              cover
            ></v-img>
            <div v-else class="card-back"></div>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      cards: [],
      flippedCards: [],
      lockBoard: false,
      images: [
'AugustoPontes.jpeg',
'ErikaVieirade.jpg',
'GustavoAlmeidade.jpg',
'JandersonSantos.jpeg',
'JoaoPauloAzevedoBaia.jpeg',
'KessiaCarvalho.jpeg',
'RicardoSilva.jpeg',
'ViniciusPaiva.jpg',
'Emanuel.jpg',
'GabrielAndradede.webp',
'IsabelleMedeiros.webp',
'JéssicaSoares.jpeg',
'JoaoPedroBarbosaAlves.jpg',
'MiguelVitorLopes.jpg',
'SadracTranquilino.webp'
      ]
    };
  },
  created() {
    this.initializeGame();
  },
  methods: {
    initializeGame() {
      const duplicatedImages = [...this.images, ...this.images];
      this.cards = duplicatedImages
        .map(image => ({
          image,
          isFlipped: false,
          isMatched: false
        }))
        .sort(() => Math.random() - 0.5);
    },
    flipCard(index) {
      if (this.lockBoard || this.cards[index].isFlipped || this.cards[index].isMatched) return;

      this.cards[index].isFlipped = true;
      this.flippedCards.push(index);

      if (this.flippedCards.length === 2) {
        this.checkForMatch();
      }
    },
    checkForMatch() {
      const [firstIndex, secondIndex] = this.flippedCards;
      const firstCard = this.cards[firstIndex];
      const secondCard = this.cards[secondIndex];

      if (firstCard.image === secondCard.image) {
        firstCard.isMatched = true;
        secondCard.isMatched = true;
      } else {
        this.lockBoard = true;
        setTimeout(() => {
          firstCard.isFlipped = false;
          secondCard.isFlipped = false;
          this.lockBoard = false;
        }, 1000);
      }

      this.flippedCards = [];
    }
  }
};
</script>

<style scoped>
.memory-card {
  cursor: pointer;
  border: 2px solid #ddd;
  transition: transform 0.3s;
  position: relative;
}
.memory-card.flipped {
  transform: rotateY(180deg);
}
.card-back {
  background-color: #9e9e9e;
  height: 100%;
  width: 100%;
}
</style>

