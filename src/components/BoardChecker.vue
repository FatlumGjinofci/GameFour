<template>
  <transition
    @enter="enter"
    :css="false">
    <circle
      :cx="centerX"
      :cy="centerY"
      :r="checkerRadius"
      :fill="adjustedColor"
      :fill-opacity="opacity"
      />
  </transition>
</template>

<script>
const OVER = 'over';
export default {
  props: ['checker', 'cellSize', 'rowCount', 'checkerRadius', 'status'],

  data() {
    return {
      colorHexes: {
        red: '#FC7E69',
        black: '#254689',
        green: 'green',
      },
    };
  },

  computed: {
    row() { return this.checker.row; },
    col() { return this.checker.col; },
    color() { return this.checker.color; },
    isWinner() { return this.checker.isWinner; },

    adjustedColor() {
      return this.colorHexes[this.color];
    },
    
    opacity() {
      return (this.status === OVER && !this.isWinner) ? 0.25 : 1.0; 
    },

    centerX() {
      return (this.cellSize / 2);
    },

    centerY() {
      return (this.cellSize / 2) + (this.cellSize * (this.rowCount - 1 - this.row));
    },

    fromY() {
      return -1 * (this.centerY + this.cellSize);
    },

    destY() {
      return 0;
    },

    percentage() {
      return (this.rowCount - this.row) / this.rowCount;
    },

    duration() {
      return 0.2 + 0.4 * this.percentage;
    },
  },

  methods: {
    enter(el, done) {
      const fromParams = { y: this.fromY };
      const destParams = {
        y: this.destY,
        // ease: Bounce.easeOut,
        onComplete: () => {
          this.$emit('land');
          done();
        },
      };

      return el, this.duration, fromParams, destParams;
    },
  },
}
</script>

<style>

</style>