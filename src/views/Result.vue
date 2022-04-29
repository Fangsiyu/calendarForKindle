
<template>
  <el-row>
    <div class="content">
      <div v-for="(date, i) in dateList" :key="i" class="page-a5">
        <div class="text">
          <h5 class="year">{{ date.year }}</h5>
          <h3>
            {{ date.month }} - {{ date.day }}
            <span class="tips"
              >{{ date.lunar.gzYear }}年-{{ date.lunar.gzMonth }}月-{{
                date.lunar.gzDay
              }}日-{{ date.lunar.IMonthCn }}{{ date.lunar.IDayCn }}</span
            >
          </h3>
          <h5 class="year-emoji">{{ date.lunar.AnimalEmoji }}</h5>
          <!-- <h5>
            {{ date.lunar.Animal }}
          </h5> -->
        </div>
        <h6 class="footer">Calendar For Kindle By Xiaoxin</h6>
      </div>
    </div>
  </el-row>
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
a {
  color: #42b983;
}
.content {
  margin: 0 auto;
}
.page-a5 {
  position: relative;
//   width: 148mm;
//   height: 210mm;
  // border: 1px solid #000;
  border-radius: 20px;
  // display: flex;
  // align-items: center;
  font-weight: bold;
  page-break-before: always;
  > .text {
    margin: 0 auto;
    width: 140mm;
    height: 200mm;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    > h3 {
      display: block;
      margin-top: 10mm;
      width: 90%;
      font-size: 70px;
      color: #fff;
      background-color: #000;
      text-align: center;
      padding: 20mm 0;
      border-radius: 20px;
      font-family: "Courgette", cursive;
      > .tips {
        display: block;
        font-size: 14px;
        margin-top: 10mm;
      }
    }
    > h5 {
      width: 100%;
      margin: 0;
      font-size: 30px;
      text-align: center;
      color: #ccc;
    }
    >.year{
      margin-top: 10px;
    }
    >.year-emoji{
      margin-top: 10px;
    }
    > .animal {
      font-size: 20px;
    }
  }
  > .footer {
    width: 100%;
    position: absolute;
    bottom: 10mm;
    left: 0;
    right: 0;
    text-align: center;
    font-family: "Courgette", cursive;
  }
}
</style>
