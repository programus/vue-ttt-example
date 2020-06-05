<template>
<button class="square" @click="clicked()">
  {{ value }}
</button>
</template>

<script>
export default {
  name: "Square",
  props: {
    value: String
  },
  methods: {
    clicked() {
      this.$emit('put', null);
      console.log('clicked');
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
