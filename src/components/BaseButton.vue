<template>
  <button
    class="btn"
    :class="[color, { block }]"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: '',
    },
    block: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style scoped>
.btn {
  height: 70px;
  border-radius: 20px;
  padding: 0;
  font-size: 25px;
  color: #000000;
  background-color: #ffffff;
}
.block {
  width: 100%;
}
.gray {
  background-color: #d2d3da;
}
.blue {
  color: #ffffff;
  background-color: #4b5efc;
}
</style>
