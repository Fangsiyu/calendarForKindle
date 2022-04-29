<template>
  <div class="progress">
    {{ date }}
  </div>
</template>
<script>
export default {
  props: {
    date: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      date: "",
    };
  },
};
</script>

<style lang="less" scoped>
</style>