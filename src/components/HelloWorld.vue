
<template>
  <el-row>
    <div class="content">
      <div v-for="(date, i) in dateList" :key="i" class="page-a5">
        <div class="text">
          <h3>
            {{ date.month }} - {{ date.day }}
            <span class="tips"
              >{{ date.lunar.gzYear }}年-{{ date.lunar.gzMonth }}月-{{
                date.lunar.gzDay
              }}日-{{ date.lunar.IMonthCn }}{{ date.lunar.IDayCn }}</span
            >
          </h3>

          <h5 class="animal">{{ date.lunar.Animal }}{{ date.lunar.AnimalEmoji }}</h5>
          <h5>{{ date.year }}</h5>
        </div>
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
        console.log(dateArr[i]);
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
.page-a5 {
  width: 148mm;
  height: 210mm;
  border: 1px solid #000;
  border-radius: 20px;
  display: flex;
  align-items: center;
  font-weight: bold;
  page-break-before: always;
  line-height: 2;
  > .text {
    width: 140mm;
    height: 200mm;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    > h3 {
      width: 90%;
      font-size: 60px;
      color: #fff;
      background-color: #000;
      text-align: center;
      padding: 20px;
      border-radius: 20px;
      > .tips {
        display: block;
        font-size: 14px;
      }
    }
    > h5 {
      width: 100%;
      font-size: 30px;
      text-align: center;
      margin-top: 30mm;
      color: #ccc;
    }
    >.animal{
      font-size: 40px;
    }
  }
}
</style>
