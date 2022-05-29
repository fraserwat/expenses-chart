<template>
  <section class="[ expenses ] [ bg-off-white color-dark-brown rad-20 ]">
    <h1 class="big-writing">Spending - Last 7 days</h1>
    <expense-chart />
    <div class="[ totals ] [ flex-between ]">
      <p class="[ total-this-month ] [ color-med-brown ]">
        Total this month
        <span class="color-dark-brown big-writing">
          ${{ thisMonth }}
        </span>
      </p>
      <p class="[ from-last-month ] [ color-med-brown ]">
        <span class="color-dark-brown weight-bold">
          {{ calculateMonthlyIncrease() }}%
        </span>from last month
      </p>
    </div>
  </section>
</template>

<script>
import ExpenseChart from './ExpenseChart.vue';

export default {
  name: 'Expenses',
  components: { ExpenseChart },
  props: ['thisMonth', 'lastMonth'],
  methods: {
    calculateMonthlyIncrease() {
      const increaseCalc = 1 - (Number(this.lastMonth) / Number(this.thisMonth));
      return (increaseCalc * 100).toFixed(1);
    },
  },
};
</script>
