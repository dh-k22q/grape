<template>
  <div id="app">
    <!-- 대기 화면 -->
    <WaitPage
      v-if="gameState === 'waiting'"
      @startGame="startGame"
    />

    <!-- 게임 진행 화면 -->
    <PlayPage 
      v-if="gameState === 'playing'"
      ref="PlayPage"
      :score="score" 
      :combo="combo" 
      :remainingTime="remainingTime"
      @restart-game="gameState='waiting'"
    />
    
    <!-- 종료 화면 -->
     <FinishPage 
      v-if="gameState === 'finished'"
      :score="score" 
      :combo="combo" 
     />

  </div>
</template>

<script>
import WaitPage from './components/WaitPage.vue';
import PlayPage from './components/PlayPage.vue';
import FinishPage from './components/FinishPage.vue';

export default {
  name: 'App',
  data() {
    return {
      gameState: 'waiting',
      grapes: [],
      selectedGrapes: [],
      score: 0,
      combo: 0,
      remainingTime: 100,
      timer: null,
    }
  },
  components: {
    WaitPage,
    PlayPage,
    FinishPage,
  },
  methods: {

    startGame() {
      this.gameState = 'playing'
      this.$refs.PlayPage.initializeGrapes()
      this.score = 0
      this.combo = 0
      this.remainingTime = 100
      this.$refs.PlayPage.startTimer()
    },
  }
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

button {
  display: block;
  margin: 20px auto;
  padding: 12px 24px;
  font-size: 18px;
  cursor: pointer;
  background-color: #8b2be2;
  color: white;
  border: none;
  border-radius: 4px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #4a0080;
}

.waiting-screen, .finish-screen {
  text-align: center;
  padding: 50px;
}

h1 {
  color: #4a0080;
  font-size: 36px;
  margin-bottom: 30px;
}

h2 {
  color: #4a0080;
  font-size: 30px;
}
</style> 