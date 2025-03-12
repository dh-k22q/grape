<template>
    <div class="playing-screen">
        <div class="game-info">
        <div class="score">점수: {{ localScore }}</div>
        <div class="combo">콤보: {{ localCombo }}</div>
        <div class="time">남은 시간: {{ localRemainingTime }}초</div>
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
        <button @click="$emit('restart-game')">다시하기</button>

    </div>
</template>
<script>
export default {
    name: "PlayPageComponent",
    props: {
        score: Number,
        combo: Number,
        remainingTime: Number,
    },
    emits: ['update:remainingTime', 'update:combo', 'update:score', 'endGame', 'restart-game'],
    data() {
        return {
            grapes: [],
            selectedGrapes: [],
            localScore: this.score,
            localCombo: this.combo,
            localRemainingTime: this.remainingTime,
            higherCombo: 0,
        }
    },
    created() {
        this.initializeGrapes();
        this.startTimer();
    },
    beforeUnmount() {
        clearInterval(this.timer);
    },
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
        this.localCombo++
        this.localScore += Math.pow(2, this.localCombo - 1)
        this.$emit('update:combo', this.localCombo)
        this.$emit('update:score', this.localScore)
        
        if (this.localCombo > this.higherCombo) {
          this.higherCombo = this.localCombo
        }
        
        if (this.grapes.every(grape => grape.removed)) {
          this.$emit('endGame', this.higherCombo)
        }
      } else {
        this.localCombo = 0
        this.$emit('update:combo', this.localCombo)
      }
      
      this.selectedGrapes = []
    },
    
    startTimer() {
      this.timer = setInterval(() => {
        this.localRemainingTime--
        this.$emit('update:remainingTime', this.localRemainingTime)
        if (this.localRemainingTime <= 0) {
          this.$emit('endGame', this.higherCombo)
        }
      }, 1000)
    },

    endGame() {
      this.gameState = 'finished'
      console.log(this.higherCombo);
      this.$emit('update:higherCombo', this.higherCombo)
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