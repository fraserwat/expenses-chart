<template>
  <div class="[ balance ] [ flex-between bg-orange color-off-white gap-bottom-600 rad-20 ]">
    <div class="flex-col">
      <h2>My balance</h2>
      <p class="big-writing">${{ balance }}</p>
    </div>
    <img src="../assets/logo.svg" alt="Expenses Logo">
  </div>
</template>

<script>
export default {
  name: 'Balance',
  props: ['balance'],
};
</script>
