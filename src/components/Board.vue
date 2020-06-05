<template>
  <div>
    <div v-for="(_, i) of Array(3).fill(null)" class="board-row" :key="i">
      <Square
        v-for="(_, j) of Array(3).fill(null)"
        :key="j"
        :value="squares[i * 3 + j]"
        @put="putStone(i * 3 + j)"
      ></Square>
    </div>
  </div>
</template>

<script>
import Square from "./Square";

export default {
  name: "Board",
  components: {
    Square
  },
  props: {
    squares: Array
  },
  methods: {
    putStone(i) {
      this.$emit("put", i);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
