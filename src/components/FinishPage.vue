<template>
  <div class="finish-screen">
    <h2>게임 종료!</h2>
    <p>최종 점수: {{ score }}</p>
    <p>최고 콤보: {{ higherCombo }}</p>
    <button @click="$emit('restartGame')">다시하기</button>
  </div>
</template>
<script>
export default {
  name: "FinishPageComponent",
  props: {
    score: Number,
    higherCombo: Number,
  }
}
</script>
<style>
.finish-screen {
  text-align: center;
  padding: 50px;
}

h2 {
  color: #4a0080;
  font-size: 30px;
}

p {
  font-size: 24px;
  margin: 10px 0;
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
</style>