<template>
  <div class="color-med-brown" ref="chart">
    <!-- {{ loadData }} -->
    <svg ></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';
import TheData from '../assets/data.json';

export default {
  name: 'ExpenseChart',
  data() {
    return {
      loadData: TheData,
    };
  },
  methods: {
    getMaxValue() {
      return Math.max(...this.loadData.map((data) => data.amount));
    },
    preUpdateClearing() {
      const svg = d3.select('svg');
      svg
        .selectAll('*')
        .remove();
    },
    renderChart() {
      this.preUpdateClearing();
      const width = this.$refs.chart.clientWidth;
      const height = 180;

      const container = d3.select('svg')
        .style('height', `${height}px`)
        .style('width', `${width}px`);

      const xScale = d3
        .scaleBand()
        .domain(this.loadData.map((dataPoint) => dataPoint.day))
        .range([0, width])
        .padding(0.2);
      const yScale = d3
        .scaleLinear()
        .domain([0, this.getMaxValue()])
        .range([Math.ceil(height), 10]);

      // eslint-disable-next-line no-unused-vars
      const bar = container
        .selectAll('.bar')
        .data(this.loadData)
        .enter()
        .append('rect')
        .attr('class', 'bar')
        .attr('class', (data) => `${data.amount === this.getMaxValue() ? 'active' : ''} bar`)
        .attr('rx', 5)
        .attr('width', xScale.bandwidth())
        .attr('height', (data) => height - yScale(data.amount))
        .attr('x', (data) => (xScale(data.day)))
        .attr('y', (data) => yScale(data.amount));

      const xAxis = d3
        .axisBottom()
        .scale(xScale);

      container.append('g')
        .attr('transform', `translate(0, ${height})`)
        .call(xAxis);

      // container.append('g')
      //   .attr('transform', `translate(0,${height})`)
      //   .call(d3.axisBottom(xScale))
      //   .selectAll('text')
      //   .attr('transform', 'translate(-15, 15) rotate(-45)');
    },
  },
  created() {
    window.addEventListener('resize', this.renderChart);
  },
  mounted() {
    this.renderChart();
  },
  updated() {
    this.renderChart();
  },
};
</script>
