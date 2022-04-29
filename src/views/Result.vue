
<template>
  <div v-for="(date, i) in dateList" :key="i" class="page-a5">
    <div class="body">
      <h5 class="year">{{ date.year }}</h5>
      <div class="content">
        {{ date.month }} - {{ date.day }}
        <span class="tips"
          >{{ date.lunar.gzYear }}年-{{ date.lunar.gzMonth }}月-{{
            date.lunar.gzDay
          }}日-{{ date.lunar.IMonthCn }}{{ date.lunar.IDayCn }}</span
        >
      </div>
      <h5 class="year-emoji">{{ date.lunar.AnimalEmoji }}</h5>
      <!-- <h5>
            {{ date.lunar.Animal }}
          </h5> -->
    </div>
    <h6 class="footer">Calendar For Kindle By Xiaoxin</h6>
  </div>
</template>

<script>
import dayjs from "dayjs";
export default {
  data() {
    return {
      dateList: [],
    };
  },
  mounted() {
    this.getDateList();
    setTimeout(() => {
      window.print();
    }, 1000);
  },
  methods: {
    getDateList() {
      let dateArr = [];
      for (let i = 0; i < 365; i++) {
        dateArr.push({
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
.content {
  margin: 0 auto;
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
      font-size: 70px;
      color: #fff;
      background-color: #000;
      text-align: center;
      padding: 10mm 0;
      border-radius: 20px;
      font-family: "Courgette", cursive;
      > .tips {
        display: block;
        font-size: 14px;
        margin-top: 5mm;
      }
    }
    > h5 {
      width: 100%;
      margin-top: 5mm;
      font-size: 30px;
      text-align: center;
      color: #ccc;
    }
    > .year {
      margin-top: 5mm;
    }
    > .year-emoji {
      margin-top: 5mm;
    }
    > .animal {
      font-size: 20px;
    }
  }
  > .footer {
    width: 100%;
    position: absolute;
    bottom: 5mm;
    left: 0;
    right: 0;
    text-align: center;
    font-family: "Courgette", cursive;
  }
}
</style>
