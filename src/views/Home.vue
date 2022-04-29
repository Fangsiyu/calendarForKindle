<template>
  <div class="index">
    <h3>Calendar For Kindle</h3>
    <el-form :model="form" label-width="120px" label-position="left">
      <el-form-item label="生成文件名">
        <el-input v-model="form.name" placeholder="生成文件名" />
      </el-form-item>
      <el-form-item label="时间范围">
        <el-date-picker
          v-model="form.dateRange"
          type="monthrange"
          range-separator="~"
          unlink-panels
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          :shortcuts="shortcuts"
        />
      </el-form-item>
      <el-form-item label="显示农历">
        <el-switch v-model="form.calendar" />
      </el-form-item>
      <el-form-item label="年份 Emoji">
        <el-switch v-model="form.emoji" />
      </el-form-item>
      <el-form-item label="当日进度">
        <el-switch v-model="form.progress" />
      </el-form-item>
      <el-form-item label="显示水印">
        <el-switch v-model="form.watermark" />
      </el-form-item>
      <el-form-item v-show="form.watermark" label="自定义水印">
        <el-input v-model="form.watermarkText" placeholder="自定义水印" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">生成</el-button>
        <el-button>重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup>
import { reactive } from "vue";
import router from "../router";
// do not use same name with ref
const form = reactive({
  name: "Calendar For Kindle",
  region: "",
  dateRange: [],
  date2: "",
  calendar: true,
  emoji: true,
  progress: true,
  watermark: true,
  watermarkText: "Calendar For Kindle",
  desc: "",
});
const shortcuts = [
  {
    text: "一年",
    value: () => {
      const start = new Date();
      const end = new Date(new Date().getFullYear() + 1, new Date().getMonth());
      return [start, end];
    },
  },
  {
    text: "二年",
    value: () => {
      const start = new Date();
      const end = new Date(new Date().getFullYear() + 2, new Date().getMonth());
      return [start, end];
    },
  },
  {
    text: "三年",
    value: () => {
      const start = new Date();
      const end = new Date(new Date().getFullYear() + 2, new Date().getMonth());
      return [start, end];
    },
  },
];

const onSubmit = () => {
  console.log("submit!");
  router.push("/result");
};
</script>
<style lang="less" scoped>
.index {
  width: 500px;
  margin: 30px auto;
  > h3 {
    font-family: "Caveat", cursive;
    font-size: 40px;
    font-weight: 600;
    margin-bottom: 20px;
    text-align: center;
  }
}
</style>
