<template>
  <div class="time">
    <span class="yyr">{{this.y}}{{this.ytype}}年{{this.month}}月{{this.d}}日</span>
    <span class="festival">{{this.festival}}</span>
    <span class="nl">&nbsp;{{this.nltpm}}</span>
    <span class="wn">&nbsp;{{this.wn}}</span>
    <span class="hms">&nbsp;{{h}}:{{m}}:{{s}}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      y: "",
      ytype: "",
      month: "",
      d: "",
      h: "",
      m: "",
      s: "",
      wn: "",
      cYear: "",
      cMonth: "",
      cDay: "",
      nltpm: "",
      festival: ""
    };
  },
  methods: {
    setTime() {
      //获得现在时间
      var today = new Date();
      //获得时分秒 年月日
      this.h = today.getHours();
      var m = today.getMinutes();
      this.m = this.checkTime(m);
      var s = today.getSeconds();
      this.s = this.checkTime(s);
      this.y = today.getFullYear();
      this.month = today.getMonth() + 1;
      this.d = today.getDate();
      //节日处理
      // this.month = 7;
      // this.d = 16;
      if (this.month == 0 && this.d == 1) {
        this.festival = "元旦";
      }
      if (this.month == 2 && this.d == 12) {
        this.festival = "植树节";
      }
      if (this.month == 3 && this.d == 5) {
        this.festival = "清明节";
      }
      if (this.month == 4 && this.d == 1) {
        this.festival = "国际劳动节";
      }
      if (this.month == 4 && this.d == 4) {
        this.festival = "青年节";
      }
      if (this.month == 5 && this.d == 1) {
        this.festival = "国际儿童节";
      }
      if (this.month == 7 && this.d == 1) {
        this.festival = "建军节";
      }
      if (this.month == 7 && this.d == 16) {
        this.festival = "七夕情人节";
      }
      if (this.month == 9 && this.d == 1) {
        this.festival = "国庆节";
      }
      if (this.month == 11 && this.d == 24) {
        this.festival = "平安夜";
      }
      if (this.month == 11 && this.d == 25) {
        this.festival = "圣诞节";
      }
      //获得星期
      var weekday = new Array(7);
      weekday[0] = "星期日";
      weekday[1] = "星期一";
      weekday[2] = "星期二";
      weekday[3] = "星期三";
      weekday[4] = "星期四";
      weekday[5] = "星期五";
      weekday[6] = "星期六";
      this.wn = weekday[today.getDay()];
      //年份处理
      var year = parseInt(this.y); //可选：将year转换为数字类型
      var check4 = 0 == year % 4; //判断年份是否能被4整除
      var check100 = 0 == year % 100; //判断年份是否能被100整除
      var check400 = 0 == year % 400; //判断年份是否能被400整除
      var isLeap = (check4 && !check100) || check400;

      if (!!isLeap) {
        this.ytype = "(润)";
      } 
      //农历处理
      this.GetLunarDay(this.y, this.month, this.d);

      // var _this = this;
      setTimeout(
        function() {
          this.setTime();
        }.bind(this),
        1000
      );
    },
    checkTime(i) {
      if (i < 10) {
        i = "0" + i;
      }
      return i;
    },
    GetLunarDay(solarYear, solarMonth, solarDay) {
      if (solarYear < 1921 || solarYear > 2020) {
        return "";
      } else {
        solarMonth = parseInt(solarMonth) > 0 ? solarMonth - 1 : 11;
        this.e2c(solarYear, solarMonth, solarDay);
        return this.GetcDateString();
      }
    },
    e2c() {
      var TheDate =
        arguments.length != 3
          ? new Date()
          : new Date(arguments[0], arguments[1], arguments[2]);
      var total, m, n, k;
      var isEnd = false;
      var tmp = TheDate.getYear();
      if (tmp < 1900) {
        tmp += 1900;
      }
      var madd = new Array(12);
      madd[0] = 0;
      madd[1] = 31;
      madd[2] = 59;
      madd[3] = 90;
      madd[4] = 120;
      madd[5] = 151;
      madd[6] = 181;
      madd[7] = 212;
      madd[8] = 243;
      madd[9] = 273;
      madd[10] = 304;
      madd[11] = 334;
      total =
        (tmp - 1921) * 365 +
        Math.floor((tmp - 1921) / 4) +
        madd[TheDate.getMonth()] +
        TheDate.getDate() -
        38;
      if (TheDate.getYear() % 4 == 0 && TheDate.getMonth() > 1) {
        total++;
      }
      var CalendarData = new Array(100);
      CalendarData = new Array(
        0xa4b,
        0x5164b,
        0x6a5,
        0x6d4,
        0x415b5,
        0x2b6,
        0x957,
        0x2092f,
        0x497,
        0x60c96,
        0xd4a,
        0xea5,
        0x50da9,
        0x5ad,
        0x2b6,
        0x3126e,
        0x92e,
        0x7192d,
        0xc95,
        0xd4a,
        0x61b4a,
        0xb55,
        0x56a,
        0x4155b,
        0x25d,
        0x92d,
        0x2192b,
        0xa95,
        0x71695,
        0x6ca,
        0xb55,
        0x50ab5,
        0x4da,
        0xa5b,
        0x30a57,
        0x52b,
        0x8152a,
        0xe95,
        0x6aa,
        0x615aa,
        0xab5,
        0x4b6,
        0x414ae,
        0xa57,
        0x526,
        0x31d26,
        0xd95,
        0x70b55,
        0x56a,
        0x96d,
        0x5095d,
        0x4ad,
        0xa4d,
        0x41a4d,
        0xd25,
        0x81aa5,
        0xb54,
        0xb6a,
        0x612da,
        0x95b,
        0x49b,
        0x41497,
        0xa4b,
        0xa164b,
        0x6a5,
        0x6d4,
        0x615b4,
        0xab6,
        0x957,
        0x5092f,
        0x497,
        0x64b,
        0x30d4a,
        0xea5,
        0x80d65,
        0x5ac,
        0xab6,
        0x5126d,
        0x92e,
        0xc96,
        0x41a95,
        0xd4a,
        0xda5,
        0x20b55,
        0x56a,
        0x7155b,
        0x25d,
        0x92d,
        0x5192b,
        0xa95,
        0xb4a,
        0x416aa,
        0xad5,
        0x90ab5,
        0x4ba,
        0xa5b,
        0x60a57,
        0x52b,
        0xa93,
        0x40e95
      );

      for (m = 0; ; m++) {
        k = CalendarData[m] < 0xfff ? 11 : 12;
        for (n = k; n >= 0; n--) {
          if (total <= 29 + this.GetBit(CalendarData[m], n)) {
            isEnd = true;
            break;
          }
          total = total - 29 - this.GetBit(CalendarData[m], n);
        }
        if (isEnd) break;
      }
      this.cYear = 1921 + m;
      this.cMonth = k - n + 1;
      this.cDay = total;
      if (k == 12) {
        if (this.cMonth == Math.floor(CalendarData[m] / 0x10000) + 1) {
          this.cMonth = 1 - this.cMonth;
        }
        if (this.cMonth > Math.floor(CalendarData[m] / 0x10000) + 1) {
          this.cMonth--;
        }
      }
    },
    GetBit(m, n) {
      return (m >> n) & 1;
    },
    GetcDateString() {
      var tgString = "甲乙丙丁戊己庚辛壬癸";
      var dzString = "子丑寅卯辰巳午未申酉戌亥";
      var numString = "一二三四五六七八九十";
      var monString = "正二三四五六七八九十冬腊";
      var weekString = "日一二三四五六";
      var sx = "鼠牛虎兔龙蛇马羊猴鸡狗猪";
      var tmp = "";
      tmp += tgString.charAt((this.cYear - 4) % 10);
      tmp += dzString.charAt((this.cYear - 4) % 12);
      tmp += "(";
      tmp += sx.charAt((this.cYear - 4) % 12);
      tmp += ")年 ";
      if (this.cMonth < 1) {
        tmp += "(闰)";
        tmp += monString.charAt(-this.cMonth - 1);
      } else {
        tmp += monString.charAt(this.cMonth - 1);
      }
      tmp += "月";
      tmp +=
        this.cDay < 11
          ? "初"
          : this.cDay < 20 ? "十" : this.cDay < 30 ? "廿" : "三十";
      if (this.cDay % 10 != 0 || this.cDay == 10) {
        tmp += numString.charAt((this.cDay - 1) % 10);
      }
      // console.log("tmp", tmp);
      this.nltpm = tmp;
      // return tmp;
    }
  },
  mounted() {
    this.setTime();
  }
};
</script>

<style lang="scss" scoped>
.time {
  color: #fff;
  .hms {
    color: #ff763a;
    font-family: "LcdD";
    display: inline-block;
    width: 75px;
    font-size: 24px;
  }
}
</style>
