<template>
  <div class="finish-screen">
    <h2>게임 종료!</h2>
    <p>최종 점수: {{ score }}</p>
    <p>최고 콤보: {{ combo }}</p>
    <button @click="$emit('restartGame')">다시하기</button>
  </div>
</template>
<script>
export default {
  name: "FinishPageComponent",
}
</script>
<style>

</style>