<template>
	<div>
		<!--单个-->
		<div ref="date_ctrl_div">
      <!--年-->
			<div ref="gear" v-if="type=='yy'" class="gear date_yy" date-datetype="date_yy" :val="dateArr.yy" :top="top_val" :style="top_val_css" v-html="yy_itemStr" @touchstart="gearTouchStart" @touchmove="gearTouchMove" @touchend="gearTouchEnd"></div>
			<!--月-->
      <div v-if="type=='mm'" class="gear date_mm" date-datetype="date_mm" :val="dateArr.mm" :top="top_val" :style="top_val_css" v-html="mm_itemStr" @touchstart="gearTouchStart" @touchmove="gearTouchMove" @touchend="gearTouchEnd"></div>
      <!--日-->
      <div v-if="type=='dd'" class="gear date_dd" date-datetype="date_dd" :val="dateArr.dd" :top="top_val" :style="top_val_css" v-html="dd_itemStr" @touchstart="gearTouchStart" @touchmove="gearTouchMove" @touchend="gearTouchEnd"></div>
			<!--时-->
      <div v-if="type=='hh'" class="gear date_hh" date-datetype="date_hh" :val="dateArr.hh" :top="top_val" :style="top_val_css" v-html="hh_itemStr" @touchstart="gearTouchStart" @touchmove="gearTouchMove" @touchend="gearTouchEnd"></div>
      <!--分-->
      <div v-if="type=='mi'" class="gear date_mi" date-datetype="date_mi" :val="dateArr.mi" :top="top_val" :style="top_val_css" v-html="mi_itemStr" @touchstart="gearTouchStart" @touchmove="gearTouchMove" @touchend="gearTouchEnd"></div>

      <div class="date_grid">
				<div>{{itemUnit}}</div>
			</div>
		</div>
	</div>
</template>
<script>
import "../datePicker.less";
export default {
  data() {
    return {
      dateVal: "", //日期时间的值
      minY: this.minDate.yy,
      minM: 1,
      minD: 1,
      maxY: this.maxDate.yy,
      maxM: 12,
      maxD: 31,
      maxH:24,
      minH:1,
      maxMi:60,
      minMi:0,

      passY: 0,
      top_val: "0em",
      top_val_css:'',
      top_mm: 0,
      top_dd: 0,
      yy_itemStr: "",
      mm_itemStr: "",
      dd_itemStr: "",
      hh_itemStr:"",
      mi_itemStr:"",
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
    },
    timestampD:{
      default:0
    }, 
    topValM:{
      default:-1
    },
    addTopM:{
      default:0
    },
    timestampM:{
      default:0
    },
    maxDate:{
      default:function(){
        return{
          yy:0,
          mm:0,//2月
          dd:0
        }
        
      }
    },
    minDate:{
      default:function(){
        return{
          yy:0,
          mm:0,//3月
          dd:0
        }
      }
    }
  },
  watch:{
    maxD:function(){
      var top_val = parseFloat(this.top_val.replace("em", ""));
      var day;
      if(this.day==0||this.type=='dd'){
        day=this.dateArr.dd;
      }else{
        day=this.day;
      }
      var timestamp = (new Date()).valueOf();
      if(this.maxD<day){
        if(this.type=='yy'||this.type=='mm'){
          this.dateArr.dd=this.maxD;
          var top_val_add_d=(day-this.maxD)*2;
          this.$emit('getTopD',[this.dateArr.dd,top_val_add_d,timestamp]);//天的val和增加的高度
        }
      }
    },
    minD:function(){
      var top_val = parseFloat(this.top_val.replace("em", ""));
      var day;
      if(this.day==0||this.type=='dd'){
        day=this.dateArr.dd;
      }else{
        day=this.day;
      }
      var timestamp = (new Date()).valueOf();
      if(this.minD>day){
        if(this.type=='yy'||this.type=='mm'){
          this.dateArr.dd=this.minD;
          var top_val_add_d=(day-this.minD)*2;
          this.$emit('getTopD',[this.dateArr.dd,top_val_add_d,timestamp]);//天的val和增加的高度
        }
      }
    },
    timestampD:function(){//时间戳变化则代表是新的月份对应日期
      if(this.type=='dd'){
        var top_val = parseFloat(this.top_val.replace("em", ""));
        this.top_val=(top_val+this.addTopD)+'em';
        this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
        this.dateArr.dd=this.topValD;
        this.maxD=this.topValD;
        this.$emit('getVal',this.dateArr.dd);
      }
    },
    maxM:function(){
      var month;
      if(this.month==-1||this.type=='mm'){
          month=this.dateArr.mm;
      }else{
        month=this.month;
      }
      var timestamp = (new Date()).valueOf();
      if(this.maxM-1<month){
        if(this.type=='yy'){
          this.dateArr.mm=this.maxM-1;
          var top_val_add_m=(month-(this.maxM-1))*2;
          this.$emit('getTopM',[this.dateArr.mm,top_val_add_m,timestamp])
        }
      }
    },
    minM:function(){
      var month;
      if(this.month==-1||this.type=='mm'){
          month=this.dateArr.mm;
      }else{
        month=this.month;
      }
      var timestamp = (new Date()).valueOf();
      if(this.minM-1>month){
        if(this.type=='yy'){
          this.dateArr.mm=this.minM-1;
          var top_val_add_m=(month-(this.minM-1))*2;
          this.$emit('getTopM',[this.dateArr.mm,top_val_add_m,timestamp])
        }
      }
    },
    timestampM:function(){//时间戳变化则代表是新的月份对应日期
      if(this.type=='mm'){
        var top_val = parseFloat(this.top_val.replace("em", ""));
        this.top_val=(top_val+this.addTopM)+'em';
        this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
        this.dateArr.mm=this.topValM;
        if(this.addTopM>0){
          this.maxM=this.topValM+1;
        }else{
          this.minM=this.topValM+1;
        }
        this.$emit('getVal',this.dateArr.mm);
      }
    },
  },
  mounted() {
    this.dateTimeCtrlInit();
  },
  methods: {
    dateTimeCtrlInit: function() {
      var that = this;
      if(that.dateArr.yy<=that.minDate.yy){//当前年份小于最小年份
        that.dateArr.yy=that.minDate.yy;
        if(that.dateArr.mm<=that.minDate.mm){
          that.dateArr.mm=that.minDate.mm;
          if(that.dateArr.dd<=that.minDate.dd){
            that.dateArr.dd=that.minDate.dd;
          }
        }
      }
      if(that.dateArr.yy>=that.maxDate.yy){//当前年份大于最大年份
        that.dateArr.yy=that.maxDate.yy;
        if(that.dateArr.mm<=that.maxDate.mm){
          that.dateArr.mm=that.maxDate.mm;
          if(that.dateArr.dd<=that.maxDate.dd){
            that.dateArr.dd=that.maxDate.dd;
          }
        }
      }
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
        if(this.minY>yyVal){
          that.dateArr.yy=this.minY;
          that.top_val = 8 + "em";
          that.top_val_css='-webkit-transform :translate3d(0,' + that.top_val + ',0)';
        }else{
          that.top_val = 8 - (yyVal - that.minY) * 2 + "em";
          that.top_val_css='-webkit-transform :translate3d(0,' + that.top_val + ',0)';
        }
        
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
          //maxM = that.maxDate.mm - 1;
        }
        //当年份到达最小值
        if (yyVal == 0) {
          console.log(111);
          minM = that.minM - 1;
          //minM = that.minDate.mm - 1;
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
        that.top_val = 8 - (mmVal - minM) * 2 + "em";
        that.top_val_css='-webkit-transform :translate3d(0,' + that.top_val + ',0)';
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
        this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
      } else if (this.type == "hh") {
        itemStr = "";

        //得到时间的值
        var hhVal = parseInt(that.dateArr.hh);
        for (var p = 0; p <= that.maxH - 1; p++) {
          itemStr += "<div class='tooth'>" + (that.minH + p) + "</div>";
        }
        that.hh_itemStr = itemStr;

        var top = Math.floor(parseFloat(that.top_val));
        that.top_val = 8 - (hhVal - that.minH) * 2 + "em";
        this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
      }else if (this.type == "mi") {
        itemStr = "";

        //得到时间的值
        var miVal = parseInt(that.dateArr.mi);
        for (var p = 0; p <= that.maxMi; p++) {
          itemStr += "<div class='tooth'>" + (that.minMi + p) + "</div>";
        }
        that.mi_itemStr = itemStr;

        var top = Math.floor(parseFloat(that.top_val));
        that.top_val = 8 - (miVal - that.minMi) * 2 + "em";
        this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
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
        //move = v * a * time;
        time = 500 + time * a;
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
      }else if(this.type=='hh'){
        this.$emit('getVal',this.dateArr.hh);
      }else if(this.type=='mi'){
        this.$emit('getVal',this.dateArr.mi);
      }
    },
    setStyle: function(move, type, time) {
      console.log(this.type);
      var fontS = parseInt(
        window.getComputedStyle(this.$refs.date_ctrl_div).fontSize
      );
      //单位转换成em
      move = move / fontS;
      var heightMax = 8;
      var heightMin = 0;

      //计算最大高度和最小高度
      if (this.type == "yy") {
        heightMin = 8-(this.maxY - this.minY)*2;
      } else if (this.type == "mm") {
        heightMin = heightMax-(this.maxM-1)*2;
        heightMax = 8-(this.minM-1)*2;
      }else if(this.type == "dd"){
        heightMin = heightMax-(this.maxD-1)*2;
        heightMax = 8-(this.minD-1)*2;
      }else if(this.type == "hh"){
        heightMin = heightMax-(this.maxH-1)*2;
        heightMax = 8-(this.minH-1)*2;
      }else if(this.type == "mi"){
        heightMin = heightMax-(this.maxMi)*2;
        heightMax = 8;
      }

      //set val
      var top_val = parseFloat(this.top_val.replace("em", ""));
      if (type == "end") {
        if (top_val >= heightMax) {
          this.top_val = heightMax + "em";
          this.top_val_css='transition: transform '+time+'ms ease-out;'+'-webkit-transform :translate3d(0,' + this.top_val + ',0)';
        }else if(top_val <= heightMin){
          this.top_val = heightMin + "em";
          this.top_val_css='transition: transform '+time+'ms ease-out;'+'-webkit-transform :translate3d(0,' + this.top_val + ',0)';
        }else {
          top_val = Math.ceil(top_val);
          if (top_val % 2 != 0) {
            top_val = top_val + 1;
          }
          this.top_val = top_val + "em";
          this.top_val_css='transition: transform '+time+'ms ease-out;'+'-webkit-transform :translate3d(0,' + this.top_val + ',0)';
        }
      } else {
        if (top_val > heightMin && top_val < heightMax) {
          this.top_val = move + top_val + "em";
          this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
        } else if (top_val < heightMin) {
          this.top_val = heightMin + "em";
          this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
        } else if (top_val > heightMax) {
         
          this.top_val = heightMax + "em";
          this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
        } else if (top_val == heightMin) {
          
          if (move + top_val > heightMin) {
            
            this.top_val = move + top_val + "em";
            this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
          } else {
            
            this.top_val = heightMin + "em";
            this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
          }
        } else if (top_val == heightMax) {
          
          if (move + top_val < heightMax) {
            
            this.top_val = move + top_val + "em";
            this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
          } else {
            
            this.top_val = heightMax + "em";
            this.top_val_css='-webkit-transform :translate3d(0,' + this.top_val + ',0)';
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
      }else if(this.type=='hh'){
        for (var k = this.minH; k <= this.maxH; k++) {
          array.push(k);
        }
        i = Math.abs(top_val - heightMax) / 2;
        this.dateArr.hh = array[i];
      }else if(this.type=='mi'){
        for (var k = this.minMi; k <= this.maxMi; k++) {
          array.push(k);
        }
        i = Math.abs(top_val - heightMax) / 2;
        this.dateArr.mi = array[i];
      }
      
      //日期最大最小值
      var year;
      var month;
      var day;
      
      if(this.year==0||this.type=='yy'){
        year=this.dateArr.yy;
      }else{
        year=this.year;
      }
      if(this.month==-1||this.type=='mm'){
        month=this.dateArr.mm;
      }else{
        month=this.month;
      }
      if(this.day==0||this.type=='dd'){
        day=this.dateArr.dd;
      }else{
        day=this.day;
      }


      if(this.type=="yy"){
        if(year==this.maxDate.yy){//年是最大年限
          this.maxM=this.maxDate.mm+1;
          this.minD=1;
          if(month==this.maxDate.mm){
            this.maxD=this.maxDate.dd;
          }else{
            var maxMonthDays = this.calcDays(year,month);
            this.maxD=maxMonthDays; 
          }
        }else if(year==this.minDate.yy){//年是最小年限
          this.minM=this.minDate.mm+1;
          this.maxM=12;
          if(this.minM!=this.dateArr.mm){
              var timestamp = (new Date()).valueOf();
              this.dateArr.mm=this.minM-1;
              var top_val_add_m=(month-this.minM+1)*2;
              this.$emit('getTopM',[this.dateArr.mm,top_val_add_m,timestamp]);//天的val和增加的高度
            }
          if(month==this.minDate.mm){
            this.minD=this.minDate.dd;
            if(this.minD!=this.dateArr.dd){
              var timestamp = (new Date()).valueOf();
              this.dateArr.dd=this.minD;
              var top_val_add_d=(day-this.minD)*2;
              this.$emit('getTopD',[this.dateArr.dd,top_val_add_d,timestamp]);//天的val和增加的高度
            }
          }
        }else{
          this.maxM=12;
          this.minM=1;
          var maxMonthDays = this.calcDays(year,month);
          this.maxD=maxMonthDays; 
        }
      }
      if(this.type=='mm'){
        if(year==this.maxDate.yy){//年是最大年限
          this.maxM=this.maxDate.mm+1;
          this.minM=1;
          if(month==this.maxDate.mm){
            this.maxD=this.maxDate.dd;
          }else{
            var maxMonthDays = this.calcDays(year,month);
            this.maxD=maxMonthDays; 
          }
        }else if(year==this.minDate.yy){//年是最小年限
          this.maxM=12;
          this.minM=this.minDate.mm+1;
          if(month==this.minDate.mm){
            this.minD=this.minDate.dd;
          }
        }else{
          this.maxM=12;
          this.minM=1;
          var maxMonthDays = this.calcDays(year,month);
          this.maxD=maxMonthDays; 
        }
      }
      if(this.type=="dd"){
        if(year==this.maxDate.yy&&month==this.maxDate.mm){
          this.maxD=this.maxDate.dd;
        }else if(year==this.minDate.yy&&month==this.minDate.mm){
          this.minD=this.minDate.dd;
          var maxMonthDays = this.calcDays(year,month);
          this.maxD=maxMonthDays; 
        }else{
          var maxMonthDays = this.calcDays(year,month);
          this.maxD=maxMonthDays; 
        }
      }
      
    }
  }
}
</script>


