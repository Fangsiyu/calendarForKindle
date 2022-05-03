<template>
  <div :style="{ width: width + '%' }" class="c-progress">
    <div :style="{ width: rate + '%' }" :title="icon" class="inner">
      <span v-if="rate < 99.99">{{ rate }}%</span>
      <span v-else>⭐️⭐️⭐️⭐️⭐️</span>
    </div>
  </div>
</template>
<script>
import dayjs from "dayjs";
export default {
  props: {
    date: {
      type: Date,
      default: new Date(),
    },
    width: {
      type: [Number, String],
      default: 100,
    },
    icon: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      rate: 0,
    };
  },
  watch: {
    date: {
      handler(val) {
        this.updateRate();
      },
      immediate: true,
    },
  },
  computed: {
    currentDate() {
      return this.date;
    },
  },
  mounted() {
    this.updateRate();
  },
  methods: {
    updateRate() {
      let totalDays = this.isLeadYear(dayjs(this.currentDate).format('YYYY')) ? 366 : 365;
      this.rate = ((dayjs(this.currentDate).dayOfYear() / totalDays) * 100).toFixed(
        5
      );
    },
    isLeadYear(year) {
      return (year % 4 == 0 && year % 100 !== 0) || year % 400 == 0;
    },
  },
};
</script>

<style lang="less" scoped>
.c-progress {
  margin: 0 auto;
  width: 100%;
  height: 4mm;
  line-height: 4mm;
  border-radius: 2mm;
  background-color: #ccc;
  > .inner {
    position: relative;
    padding: 0 1mm;
    min-width: 4mm;
    height: 4mm;
    line-height: 4mm;
    border-radius: 2mm;
    background-color: #000;
    font-size: 12px;
    color: #fff;
    text-align: right;
    white-space: nowrap;
    text-shadow: 0 0 1px #000;
  }
}
</style>