<template>
  <div class="[ container ] [ max-width-540 ]">
    <balance :balance="921.48"/>
    <expenses :this-month="478.33" :last-month="467.00" />
  </div>
</template>

<script>
import Expenses from './components/Expenses.vue';
import Balance from './components/Balance.vue';

export default {
  name: 'App',
  components: {
    Expenses,
    Balance,
  },
};
</script>
