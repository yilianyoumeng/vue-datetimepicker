<template>
	<div>
		<!--单个-->
		<div ref="date_ctrl_div">
      <!--年-->
			<div v-if="type=='yy'" class="gear date_yy" data-datetype="date_yy" :val="dateArr.yy" :top="top_val" :style="'-webkit-transform :translate3d(0,' + top_val + ',0)'" v-html="yy_itemStr" @touchstart="gearTouchStart" @touchmove="gearTouchMove" @touchend="gearTouchEnd"></div>
			<!--月-->
      <div v-if="type=='mm'" class="gear date_mm" data-datetype="date_mm" :val="dateArr.mm" :top="top_val" :style="'-webkit-transform :translate3d(0,' + top_val + ',0)'" v-html="mm_itemStr" @touchstart="gearTouchStart" @touchmove="gearTouchMove" @touchend="gearTouchEnd"></div>
      <!--日-->
      <div v-if="type=='dd'" class="gear date_dd" data-datetype="date_dd" :val="dateArr.dd" :top="top_val" :style="'-webkit-transform :translate3d(0,' + top_val + ',0)'" v-html="dd_itemStr" @touchstart="gearTouchStart" @touchmove="gearTouchMove" @touchend="gearTouchEnd"></div>
			<div class="date_grid">
				<div>{{itemUnit}}</div>
			</div>
		</div>
	</div>
</template>
<script>
import "../dataPicker.less";
export default {
  data() {
    return {
      dataVal: "", //日期时间的值
      minY: 2000,
      minM: 1,
      minD: 1,
      maxY: 2018,
      maxM: 12,
      maxD: 31,
      passY: 0,
      top_val: "0em",
      top_mm: 0,
      top_dd: 0,
      yy_itemStr: "",
      mm_itemStr: "",
      dd_itemStr: "",
      dateArr: {
        yy: new Date().getFullYear(),
        mm: new Date().getMonth(),
        dd: new Date().getDate(),
        hh: new Date().getHours(),
        mi: new Date().getMinutes()
      },
      finger: {}
    };
  },
  props: {
    itemUnit: {
      default: ""
    },
    type: {
      default: ""
    },
    year:{
      default: 0
    },
    month:{
      default: -1
    },
    day:{
      default: 0
    },
    topValD:{
      default:0
    },
    addTopD:{
      default:0
    }
    // placeholder: {
    //   default: ""
    // },
  },
  watch:{
    maxD:function(){
      console.log(this.type);
      var top_val = parseFloat(this.top_val.replace("em", ""));
      console.log('@@@@@@');
      //console.log(month);
      console.log(this.maxD);
      var day;
      console.log(this.day);
      if(this.day==0){
        day=this.dateArr.dd;
      }else{
        day=this.day;
      }
      console.log(day);
      if(this.maxD<day){
        if(this.type=='yy'||this.type=='mm'){
          this.dateArr.dd=this.maxD;
          var top_val_add_d=(day-this.maxD)*2
          this.$emit('getTopD',[this.dateArr.dd,top_val_add_d]);//天的val和增加的高度
        }
      }
    },
    addTopD:function(){
      console.log(this.type);
      if(this.type=='dd'){
        var top_val = parseFloat(this.top_val.replace("em", ""));
        console.log('天儿！');
        this.top_val=(top_val+this.addTopD)+'em';
        this.dateArr.dd=this.topValD;
      }
    }
  },
  mounted() {
    this.dateTimeCtrlInit();
  },
  methods: {
    dateTimeCtrlInit: function() {
      var that = this;
      that.setDateGearTooth();
    },
    //初始化
    setDateGearTooth: function() {
      var that = this;
      var itemStr = "";
      //得到年份的值
      var yyVal = parseInt(that.dateArr.yy); //2018
      if (this.type == "yy") {
        that.passY = that.maxY - that.minY + 1;
        //p 当前节点前后需要展示的节点个数
        for (var p = 0; p <= that.passY - 1; p++) {
          itemStr += "<div class='tooth'>" + (that.minY + p) + "</div>";
        }
        that.yy_itemStr = itemStr;

        var top = Math.floor(parseFloat(that.top_val));
        that.top_val = 8 - (yyVal - that.minY) * 2 + "em";
      } else if (this.type == "mm") {
        itemStr = "";
        that.passY = that.maxY - that.minY + 1;
        //得到月份的值
        var mmVal = parseInt(that.dateArr.mm);
        var maxM = 11;
        var minM = 0;
        //当年份到达最大值
        if (yyVal == that.passY - 1) {
          maxM = that.maxM - 1;
        }
        //当年份到达最小值
        if (yyVal == 0) {
          minM = that.minM - 1;
        }
        //p 当前节点前后需要展示的节点个数
        for (var p = 0; p < maxM - minM+1; p++) {
          itemStr += "<div class='tooth'>" + (minM + p + 1) + "</div>";
        }
        that.mm_itemStr = itemStr;
        if (mmVal > maxM) {
          mmVal = maxM;
          that.dateArr.mm = mmVal;
        } else if (mmVal < minM) {
          mmVal = maxM;
          that.dateArr.mm = mmVal;
        }
        console.log((mmVal - minM) * 2)
        that.top_val = 8 - (mmVal - minM) * 2 + "em";
      } else if (this.type == "dd") {
        itemStr = "";

        //得到日期的值
        var ddVal = parseInt(that.dateArr.dd);
        //返回月份的天数
        var maxMonthDays = that.calcDays(yyVal, mmVal);
        //p 当前节点前后需要展示的节点个数
        var maxD = maxMonthDays;
        var minD = 1;
        //当年份月份到达最大值
        if (yyVal == that.passY - 1 && that.maxM == mmVal + 1) {
          maxD = that.maxD - 1;
        }
        //当年、月到达最小值
        if (yyVal == 0 && that.minM == mmVal + 1) {
          minD = that.minD;
        }
        for (var p = 0; p < maxD - minD + 1; p++) {
          itemStr += "<div class='tooth'>" + (minD + p) + "</div>";
        }
        that.dd_itemStr = itemStr;
        if (ddVal > maxD) {
          ddVal = maxD;
          that.dateArr.dd = ddVal;
        } else if (ddVal < minD) {
          ddVal = minD;
          that.dateArr.dd = ddVal;
        }
        that.top_val = 8 - (ddVal - minD) * 2 + "em";
      }
    },
    //求月份最大天数
    calcDays: function(year, month) {
      var that = this;
      if (month == 1) {//2月
        year += that.minY;
        if (
          (year % 4 == 0 && year % 100 != 0) ||
          (year % 400 == 0 && year % 4000 != 0)
        ) {
          return 29;
        } else {
          return 28;
        }
      } else {
        if (month == 3 || month == 5 || month == 8 || month == 10) {//4月，6月，9月，11月
          return 30;
        } else {
          return 31;
        }
      }
    },
    //触摸开始
    gearTouchStart: function(event) {
      let finger = event.changedTouches[0];
      this.finger.startY = finger.pageY;
      this.finger.lastY = finger.pageY;
      this.finger.startTime = event.timestamp || Date.now();
      event.preventDefault();
    },
    //手指移动
    gearTouchMove: function(event) {
      var finger = event.changedTouches[0];
      this.finger.lastTime = event.timestamp || Date.now();
      /* 设置css */
      let move = finger.pageY - this.finger.lastY;
      this.finger.lastY = finger.pageY;
      
      this.setStyle(move);
      event.preventDefault();
    },
    //离开屏幕
    gearTouchEnd: function(event) {
      console.log("离开屏幕");
      let finger = event.changedTouches[0];
      this.finger.lastY = finger.pageY;
      this.finger.lastTime = event.timestamp || Date.now();

      let move = this.finger.lastY - this.finger.startY;
      /* 计算速度 */
      /* 速度计算说明
         * 当时间小于300毫秒 最后的移动距离等于 move + 减速运动距离
         * */

      let time = this.finger.lastTime - this.finger.startTime;
      let v = move / time;
      /* 减速加速度a */
      let a = 1.8;
      /* 设置css */
      if (time <= 300) {
        move = v * a * time;
        time = 1000 + time * a;
        this.setStyle(move, "end", time);
      } else {
        this.setStyle(move, "end");
      }

      if(this.type=='yy'){
        this.$emit('getVal',this.dateArr.yy);
      }else if(this.type=='mm'){
        this.$emit('getVal',this.dateArr.mm);
      }else if(this.type=='dd'){
        this.$emit('getVal',this.dateArr.dd);
      }
      console.log(this.maxD);
    },
    
    setStyle: function(move, type, time) {
      var fontS = parseInt(
        window.getComputedStyle(this.$refs.date_ctrl_div).fontSize
      );
      //单位转换成em
      move = move / fontS;
      var heightMax = 8;
      var heightMin = 0;

      //判断是年月日
      if (this.type == "yy") {
        heightMin = 8-(this.maxY - this.minY)*2;
      } else if (this.type == "mm") {
        heightMin = 8-(this.maxM - this.minM)*2;
      }else if(this.type == "dd"){
        heightMin = 8-(this.maxD - this.minD)*2;
      }

      //set top_val
      var top_val = parseFloat(this.top_val.replace("em", ""));
      if (type == "end") {
        if (top_val >= heightMax) {
          this.top_val = heightMax + "em";
        }else if(top_val <= heightMin){
          this.top_val = heightMin + "em";
        }else {
          top_val = Math.ceil(top_val);
          if (top_val % 2 != 0) {
            top_val = top_val + 1;
          }
          this.top_val = top_val + "em";
        }
      } else {
        if (top_val > heightMin && top_val < heightMax) {
          this.top_val = move + top_val + "em";
        } else if (top_val < heightMin) {
          this.top_val = heightMin + "em";
        } else if (top_val > heightMax) {
          this.top_val = heightMax + "em";
        } else if (top_val == heightMin) {
          if (move + top_val > heightMin) {
            this.top_val = move + top_val + "em";
          } else {
            this.top_val = heightMin + "em";
          }
        } else if (top_val == heightMax) {
          if (move + top_val < heightMax) {
            this.top_val = move + top_val + "em";
          } else {
            this.top_val = heightMax + "em";
          }
        }
      }
      var array = [];
      var i = 0;
      if(this.type=='yy'){
        for (var k = this.minY; k <= this.maxY; k++) {
          array.push(k);
        }
        i = Math.abs(top_val - heightMax) / 2;
        this.dateArr.yy = array[i];
      }else if(this.type=='mm'){
        for (var k = this.minM-1; k <= this.maxM-1; k++) {
          array.push(k);
        }
        i = Math.abs(top_val - heightMax) / 2;
        this.dateArr.mm = array[i];
      }else if(this.type=='dd'){
        for (var k = this.minD; k <= this.maxD; k++) {
          array.push(k);
        }
        i = Math.abs(top_val - heightMax) / 2;
        this.dateArr.dd = array[i];
      }
      //月份对应天数
      var year;
      var month;
      
      if(this.year==0){
        year=this.dateArr.yy;
      }else{
        year=this.year;
      }
      if(this.month==-1){
        month=this.dateArr.mm;
      }else{
        month=this.month;
      }
      var maxMonthDays = this.calcDays(year,month);
      this.maxD=maxMonthDays;
      console.log('月份最大天数');
      console.log(month);
      console.log(this.maxD);  
    }
  }
};
</script>


