<template>
  <!-- 대기 화면 -->
  <div class="waiting-screen">
    <h1>포도 게임</h1>
    <button @click="$emit('startGame')">시작하기</button>
    <p>3개의 숫자를 합하여 홀수를 만드세요!</p>
  </div>
</template>

<script>
export default {
  name: "WaitPageComponent"
}
</script>

<style>

</style>