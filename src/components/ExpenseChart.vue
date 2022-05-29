<template>
  <div class="[ svg-container ] [ color-med-brown ]" ref="chart">
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
        .attr('class', (data) => `${data.day} bar${data.amount === this.getMaxValue() ? ' active' : ''}`)
        .attr('rx', 5)
        .attr('width', xScale.bandwidth())
        .attr('height', (data) => height - yScale(data.amount))
        .attr('x', (data) => (xScale(data.day)))
        .attr('y', (data) => yScale(data.amount))
        // event listeners
        .on('mouseover', (elem) => this.tooltipListener(elem.target.classList[0]))
        .on('mouseout', (elem) => this.tooltipListenerOff(elem.target.classList[0]))
        // accessible tooltips
        .append('title')
        .text((data) => `$${data.amount}`);

      const xAxis = d3
        .axisBottom()
        .scale(xScale);

      container.append('g')
        .attr('transform', `translate(0, ${height})`)
        .call(xAxis);

      this.renderTooltips();
    },
    tooltipListener(weekday) {
      this.$refs.chart.querySelector(`#${weekday}`).classList.add('active');
    },
    tooltipListenerOff(weekday) {
      this.$refs.chart.querySelector(`#${weekday}`).classList.remove('active');
    },
    renderTooltips() {
      const svg = this.$refs.chart.children;
      if (svg) {
        svg[0].childNodes.forEach((node) => {
          if (node.classList.contains('bar')) {
            const tooltip = document.createElement('p');
            tooltip.innerHTML = node.querySelector('title').innerHTML;
            tooltip.classList.add('tooltip');

            let cssConstructor = `transform: translate(${node.getAttribute('x')}px);`;
            cssConstructor += `top: ${node.getAttribute('y')}px;`;
            tooltip.setAttribute('style', cssConstructor);
            tooltip.setAttribute('id', node.classList[0]);
            svg[0].parentNode.appendChild(tooltip);
          }
        });
      }
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

<style scoped>
  .svg-container {
    position: relative;
  }
</style>
