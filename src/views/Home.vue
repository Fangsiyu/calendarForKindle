<template>
  <div class="title">
    <span>Calendar For Kindle</span>
  </div>
  <div class="index">
    <div class="form-box">
      <el-form
        :model="form"
        label-width="120px"
        ref="formRef"
        label-position="left"
      >
        <el-form-item label="生成文件名">
          <el-input v-model="form.name" placeholder="生成文件名" clearable />
        </el-form-item>
        <el-form-item label="时间范围">
          <el-date-picker
            v-model="form.dateRange"
            type="daterange"
            range-separator="~"
            unlink-panels
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            :shortcuts="shortcuts"
            :clearable="false"
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
          <el-input
            v-model="form.watermarkText"
            placeholder="自定义水印"
            clearable
          />
        </el-form-item>
        <el-form-item label="自动弹窗打印">
          <el-switch v-model="form.autoPrint" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">生成</el-button>
          <el-button @click="resetForm">重置</el-button>
        </el-form-item>
        <el-alert
          title="1.日期范围不建议选择过大，会影响浏览器生成 PDF 速度。"
          :closable="false"
          type="warning"
        />
        <el-alert
          title="2.PDF 利用浏览器打印功能，另存为 PDF 即可。"
          :closable="false"
          type="warning"
        />
      </el-form>
    </div>
    <div class="form-preview">
      <div class="page-a5">
        <div class="body">
          <h5 class="year">{{ result.year }}-{{ result.month }}</h5>
          <div class="content">
            {{ result.day }}
            <span v-show="form.calendar" class="tips"
              >{{ result.lunar.gzYear }}年·{{ result.lunar.gzMonth }}月·{{
                result.lunar.gzDay
              }}日·{{ result.lunar.IMonthCn }}{{ result.lunar.IDayCn }}</span
            >
          </div>
          <h5 v-show="form.emoji" class="year-emoji">
            {{ result.lunar.AnimalEmoji }}
          </h5>
          <cProgress
            v-show="form.progress"
            class="progress"
            :date="form.dateRange[0]"
            :icon="result.lunar.AnimalEmoji"
            width="80"
          ></cProgress>
        </div>

        <h6 v-show="form.watermark" class="footer">{{ form.watermarkText }}</h6>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, watchEffect } from "vue";

import router from "../router";
import dayjs from "dayjs";
import cProgress from "../components/cProgress.vue";
// do not use same name with ref
const form = reactive({
  name: "Calendar For Kindle",
  dateRange: [],
  calendar: true,
  emoji: true,
  progress: true,
  watermark: true,
  watermarkText: "Calendar For Kindle By Xiaoxin",
  desc: "",
  autoPrint: true,
});
const shortcuts = [
  {
    text: "一年",
    value: () => {
      const start = new Date();
      const end = new Date(
        new Date().getFullYear() + 1,
        new Date().getMonth(),
        new Date().getDay() + 1
      );
      return [start, end];
    },
  },
  {
    text: "二年",
    value: () => {
      const start = new Date();
      const end = new Date(
        new Date().getFullYear() + 2,
        new Date().getMonth(),
        new Date().getDay() + 1
      );
      return [start, end];
    },
  },
  {
    text: "三年",
    value: () => {
      const start = new Date();
      const end = new Date(
        new Date().getFullYear() + 3,
        new Date().getMonth(),
        new Date().getDay() + 1
      );
      return [start, end];
    },
  },
];

const result = reactive({
  year: "",
  month: "",
  day: "",
  lunar: "",
});

watchEffect(() => {
  if (form.dateRange) {
    console.log(form.dateRange);
    result.lunar = calendar.solar2lunar(
      dayjs(form.dateRange[0]).format("YYYY"),
      dayjs(form.dateRange[0]).format("MM"),
      dayjs(form.dateRange[0]).format("DD")
    );
    console.log(result.lunar);
    result.year = dayjs(form.dateRange[0]).format("YYYY");
    result.month = dayjs(form.dateRange[0]).format("MM");
    result.day = dayjs(form.dateRange[0]).format("DD");
  }
});

const onSubmit = () => {
  console.log("submit!");
  router.push({
    name: "result",
    query: {
      form: JSON.stringify(form),
    },
  });
};
const formRef = ref(null);
const resetForm = () => {
  location.reload();
};
onMounted(() => {
  form.dateRange = [
    new Date(),
    new Date(
      new Date().getFullYear() + 1,
      new Date().getMonth(),
      new Date().getDay() + 1
    ),
  ];
  result.year = dayjs(form.dateRange[0]).format("YYYY");
  result.month = dayjs(form.dateRange[0]).format("MM");
  result.day = dayjs(form.dateRange[0]).format("DD");
});
</script>
<style lang="less" scoped>
.title {
  font-family: "Caveat", cursive;
  font-size: 50px;
  font-weight: 600;
  margin: 20px 0;
  text-align: center;
  text-align: center;
  > span {
    padding-right: 5px;
    color: transparent;
    background-image: repeating-linear-gradient(
      to right,
      red,
      orange,
      yellow,
      green,
      rgb(0, 208, 159),
      blue,
      indigo,
      red
    );
    -webkit-background-clip: text;
    animation: run 10s linear infinite alternate;
  }
}
@keyframes run {
  from {
    backgroud-position: 0 800px;
  }
  to {
    background-position: 800px 0;
  }
}
.index {
  display: flex;
  max-width: 1000px;
  margin: 0 auto;
  > .form-box {
    width: 50%;
    margin: 30px auto;
  }
  > .form-preview {
    width: 50%;
    padding: 20px;
  }
}
.page-a5 {
  position: relative;
  width: 100%;
  padding: 5mm;
  width: 148mm;
  height: 210mm;
  border: 1px solid #000;
  border-radius: 20px;
  font-weight: bold;
  page-break-before: always;
  overflow: hidden;
  display: flex;
  flex-direction: row;
  align-items: center;
  > .body {
    width: 100%;
    > .content {
      display: block;
      margin-top: 5mm;
      width: 100%;
      font-size: 100px;
      color: #fff;
      font-weight: 400;
      background-color: #000;
      text-align: center;
      padding: 20mm 0;
      border-radius: 20px;
      font-family: "Orelega One", cursive;
      letter-spacing: 2mm;
      > .tips {
        display: block;
        font-size: 14px;
        margin-top: 5mm;
        font-family: "Noto Serif SC", serif;
        letter-spacing: 1mm;
      }
    }
    > h5 {
      width: 100%;
      margin-top: 5mm;
      font-size: 40px;
      text-align: center;
      color: #ccc;
    }
    > .year {
      font-size: 40px;
      margin-top: 5mm;
      // font-family: "Caveat", cursive;
      font-family: "Noto Serif SC", serif;
    }
    > .year-emoji {
      margin-top: 5mm;
    }
    > .animal {
      font-size: 20px;
    }
    > .progress {
      margin-top: 5mm;
    }
  }
  > .footer {
    width: 100%;
    position: absolute;
    bottom: 5mm;
    left: 0;
    right: 0;
    text-align: center;
    font-family: "Caveat", cursive;
  }
}
</style>
