<template>
    <div class="playing-screen">
        <div class="game-info">
        <div class="score">점수: {{ score }}</div>
        <div class="combo">콤보: {{ combo }}</div>
        <div class="time">남은 시간: {{ remainingTime }}초</div>
        </div>
        
        <div class="grape-container">
        <div 
            v-for="(grape, index) in grapes" 
            :key="index"
            :class="['grape-slot']"
        >
            <div
            v-if="!grape.removed"
            :class="['grape', { selected: selectedGrapes.includes(index) }]"
            @click="selectGrape(index)"
            >
            {{ grape.number }}
            </div>
        </div>
        </div>
        <button @click="$emit('restartGame')">다시하기</button>
    </div>
</template>
<script>
export default {
    name: "PlayPageComponent",

    methods: {
    initializeGrapes() {
      this.grapes = []
      for (let i = 0; i < 450; i++) {
        this.grapes.push({
          number: Math.floor(Math.random() * 9) + 1,
          removed: false
        })
      }
    },

    selectGrape(index) {
      if (this.selectedGrapes.includes(index)) {
        this.selectedGrapes = this.selectedGrapes.filter(i => i !== index)
      } else if (this.selectedGrapes.length < 3) {
        this.selectedGrapes.push(index)
      }

      if (this.selectedGrapes.length === 3) {
        this.checkSum()
      }
    },

    checkSum() {
      const sum = this.selectedGrapes.reduce((acc, index) => {
        return acc + this.grapes[index].number
      }, 0)

      if (sum % 2 === 1 || this.grapes.filter(grape => !grape.removed).length === 3) {
        this.selectedGrapes.forEach(index => {
          this.grapes[index].removed = true
        })
        this.combo++
        this.score += Math.pow(2, this.combo - 1)
        
        if (this.grapes.every(grape => grape.removed)) {
          this.endGame()
        }
      } else {
        this.combo = 0
      }
      
      this.selectedGrapes = []
    },
    
    startTimer() {
      this.timer = setInterval(() => {
        this.remainingTime--
        if (this.remainingTime <= 0) {
          this.endGame()
        }
      }, 1000)
    },

    endGame() {
      this.gameState = 'finished'
      clearInterval(this.timer)
    },

    restartGame() {
      this.gameState = 'waiting'
    }
    }
}
</script>
<style>
.game-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  font-size: 24px;
  font-weight: bold;
}
.grape-container {
  display: grid;
  grid-template-columns: repeat(30, 1fr);
  gap: 4px;
  background-color: #f0f0f0;
  padding: 10px;
  border-radius: 8px;
}
  .grape-slot {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.1); /* 빈 자리 표시 */
}
.grape {
  width: 100%;
  height: 100%;
  background-color: #8b2be2;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  border-radius: 50%;
  font-size: 16px;
  transition: all 0.2s ease;
}
.grape.selected {
  background-color: #4a0080;
  transform: scale(1.1);
  border: 2px solid #ffd700;
}
</style>