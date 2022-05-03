
<template>
  <div v-for="(date, i) in dateList" :key="i" class="page-a5">
    <div class="body">
      <h5 class="year">{{ date.year }} - {{ date.month }}</h5>
      {{ emoji }}
      <div class="content">
        {{ date.day }}
        <span v-show="form.calendar" class="tips"
          >{{ date.lunar.gzYear }}年·{{ date.lunar.gzMonth }}月·{{
            date.lunar.gzDay
          }}日·{{ date.lunar.IMonthCn }}{{ date.lunar.IDayCn }}</span
        >
      </div>
      <h5 v-show="form.emoji" class="year-emoji">{{ date.lunar.AnimalEmoji }}</h5>
      <cProgress
      v-show="form.progress"
        class="progress"
        :date="date.date"
        :icon="date.lunar.AnimalEmoji"
        width="80"
      ></cProgress>
    </div>

    <h6 v-show="form.watermark" class="footer">{{form.watermarkText}}</h6>
  </div>
</template>

<script>
import dayjs from "dayjs";
import cProgress from "../components/cProgress.vue";

export default {
  data() {
    return {
      dateList: [],
      emoji: "",
      form: {
        name: "",
        dateRange: [],
        calendar: true,
        emoji: true,
        progress: true,
        watermark: true,
        watermarkText: "" || 'Calendar For Kindle By Xiaoxin',
        desc: "",
      },
    };
  },
  components: {
    cProgress,
  },
  mounted() {
    console.log(JSON.parse(this.$route.query.form))
    this.form = Object.assign(this.form, JSON.parse(this.$route.query.form));
    this.init();

    // setTimeout(() => {
    //   window.print();
    // }, 1000);
  },
  methods: {
    //初始化
    init(){
      document.title = this.form.name; //设置网页标题
      this.getDateList();

    },
    getDateList() {
      let dateArr = [];
      for (let i = 0; i < 365; i++) {
        dateArr.push({
          date: dayjs().add(i, "day").toDate(),
          year: dayjs().add(i, "day").format("YYYY"),
          month: dayjs().add(i, "day").format("MM"),
          day: dayjs().add(i, "day").format("DD"),
          lunar: calendar.solar2lunar(
            dayjs().add(i, "day").format("YYYY"),
            dayjs().add(i, "day").format("MM"),
            dayjs().add(i, "day").format("DD")
          ),
        });
      }
      this.dateList = dateArr;
    },
  },
};
</script>



<style lang="less" scoped>
.page-a5 {
  position: relative;
  width: 100%;
  padding: 5mm;
  width: 148mm;
  height: 210mm;
  // border: 1px solid #000;
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
