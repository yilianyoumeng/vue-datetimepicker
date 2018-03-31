<template>
	<div>
    <input type="text" v-model="dataVal">
		<!--年月日-->
		<div class="date_ctrl slideInUp">
			<div class="date_btn_box">
				<div class="date_btn lcalendar_cancel">取消</div>
				<div class="date_btn lcalendar_finish">确定</div>
			</div>
			<div class="date_roll_mask">
				<div class="date_roll">
					<div  ref="date_ctrl_div">
						<div class="gear date_yy" data-datetype="date_yy" :val="dateArr.yy" :top="top_yy" :style="'-webkit-transform :translate3d(0,' + top_yy + ',0)'" v-html="yy_itemStr" @touchstart="gearTouchStart" @touchmove="gearTouchMove" @touchend="gearTouchEnd" ref="yy"><!---->
						</div>
						<div class="date_grid">
							<div>年</div>
						</div>
					</div>
					<div>
						<div class="gear date_mm" data-datetype="date_mm" :val="dateArr.mm" :top="top_mm" :style="'-webkit-transform :translate3d(0,' + top_mm + ',0)'" v-html="mm_itemStr">

						</div>
						<div class="date_grid">
							<div>月</div>
						</div>
					</div>
					<div>
						<div class="gear date_dd" data-datetype="date_dd" :val="dateArr.dd" :top="top_dd" :style="'-webkit-transform :translate3d(0,' + top_dd + ',0)'" v-html="dd_itemStr">

						</div>
						<div class="date_grid">
							<div>日</div>
						</div>
					</div>
				</div>
			</div>
		</div>
		<!--年月日时分-->
		<!-- <div class="date_ctrl slideInUp">
			<div class="date_btn_box">
				<div class="date_btn lcalendar_cancel">取消</div>
				<div class="date_btn lcalendar_finish">确定</div>
			</div>
			<div class="date_roll_mask">
				<div class="datetime_roll">
					<div>
						<div class="gear date_yy" data-datetype="date_yy" :val="dateArr.yy"></div>
						<div class="date_grid">
							<div>年</div>
						</div>
					</div>
					<div>
						<div class="gear date_mm" data-datetype="date_mm" :val="dateArr.mm"></div>
						<div class="date_grid">
							<div>月</div>
						</div>
					</div>
					<div>
						<div class="gear date_dd" data-datetype="date_dd" :val="dateArr.dd"></div>
						<div class="date_grid">
							<div>日</div>
						</div>
					</div>
					<div>
						<div class="gear time_hh" data-datetype="time_hh"></div>
						<div class="date_grid">
							<div>时</div>
						</div>
					</div>
					<div>
						<div class="gear time_mm" data-datetype="time_mm"></div>
						<div class="date_grid">
							<div>分</div>
						</div>
					</div>
				</div>
			</div>
		</div> -->
		<!--时分秒-->
		<!-- <div class="date_ctrl slideInUp">
			<div class="date_btn_box">
				<div class="date_btn lcalendar_cancel">取消</div>
				<div class="date_btn lcalendar_finish">确定</div>
			</div>
			<div class="date_roll_mask">
				<div class="time_roll">
					<div>
						<div class="gear time_hh" data-datetype="time_hh"></div>
						<div class="date_grid">
							<div>时</div>
						</div>
					</div>
					<div>
						<div class="gear time_mm" data-datetype="time_mm"></div>
						<div class="date_grid">
							<div>分</div>
						</div>
					</div>
					<div>
						<div class="gear time_ss" data-datetype="time_ss"></div>
						<div class="date_grid">
							<div>秒</div>
						</div>
					</div>
				</div>
			</div>
		</div> -->
	</div>
</template>
<script>
import "../dataPicker.less";
export default {
  data() {
    return {
      dataVal:'',//日期时间的值
      minY: 2011,
      minM: 1,
      minD: 1,
      maxY: 2019,
      maxM: 12,
      maxD: 31,
      passY: 0,
      top_yy: 0,
      top_mm: 0,
      top_dd: 0,
      yy_itemStr: "",
      mm_itemStr: "",
      dd_itemStr: "",
      dateArr: {
        yy: new Date().getFullYear(),
        mm: new Date().getMonth(),
        dd: new Date().getDate() - 1,
        hh: new Date().getHours(),
        mi: new Date().getMinutes()
			},
			finger:{}
    };
  },
  props: {
    // isSelectItem: {
    //   default: false
    // },
    // placeholder: {
    //   default: ""
    // },
  },
  mounted() {
    this.dateTimeCtrlInit();
  },
  methods: {
    dateTimeCtrlInit: function() {
      var that = this;
      that.setDateGearTooth();
    },
    setDateGearTooth: function() {
      var that = this;
      that.passY = that.maxY - that.minY + 1;
      //var date_yy = that.gearDate.querySelector(".date_yy");
      var itemStr = "";
      //if (date_yy && date_yy.getAttribute("val")) {
      //得到年份的值
      var yyVal = parseInt(that.dateArr.yy);//2018
      //p 当前节点前后需要展示的节点个数
      for (var p = 0; p <= that.passY - 1; p++) {
        itemStr += "<div class='tooth'>" + (that.minY + p) + "</div>";
      }
      that.yy_itemStr = itemStr;

      var top = Math.floor(parseFloat(that.top_yy));
      that.top_yy = 8 - (yyVal - that.minY) * 2 + "em";
      itemStr = "";
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
      for (var p = 0; p < maxM - minM + 1; p++) {
        itemStr += "<div class='tooth'>" + (minM + p + 1) + "</div>";
      }
      that.mm_itemStr = itemStr;
      if (mmVal > maxM) {
        mmVal = maxM;
        //date_mm.setAttribute("val", mmVal);
        that.dateArr.mm = mmVal;
      } else if (mmVal < minM) {
        mmVal = maxM;
        //date_mm.setAttribute("val", mmVal);
        that.dateArr.mm = mmVal;
      }
      that.top_mm = 8 - (mmVal - minM) * 2 + "em";
      //date_mm.style["-webkit-transform"] = 'translate3d(0,' + (8 - (mmVal - minM) * 2) + 'em,0)';
      //date_mm.setAttribute('top', 8 - (mmVal - minM) * 2 + 'em');
      //} else {
      //	return;
      //}
      //var date_dd = that.gearDate.querySelector(".date_dd");
      //if (date_dd && date_dd.getAttribute("val")) {
      itemStr = "";
      //得到日期的值
      var ddVal = parseInt(that.dateArr.dd);
      //返回月份的天数
      var maxMonthDays = that.calcDays(yyVal, mmVal);
      //p 当前节点前后需要展示的节点个数
      var maxD = maxMonthDays - 1;
      var minD = 0;
      //当年份月份到达最大值
      if (yyVal == that.passY - 1 && that.maxM == mmVal + 1) {
        maxD = that.maxD - 1;
      }
      //当年、月到达最小值
      if (yyVal == 0 && that.minM == mmVal + 1) {
        minD = that.minD - 1;
      }
      for (var p = 0; p < maxD - minD + 1; p++) {
        itemStr += "<div class='tooth'>" + (minD + p + 1) + "</div>";
      }
      that.dd_itemStr = itemStr;
      if (ddVal > maxD) {
        ddVal = maxD;
        that.dateArr.dd = ddVal;
        //date_dd.setAttribute("val", ddVal);
      } else if (ddVal < minD) {
        ddVal = minD;
        that.dateArr.dd = ddVal;
        //date_dd.setAttribute("val", ddVal);
      }
      that.top_dd = 8 - (ddVal - minD) * 2 + "em";
      // date_dd.style["-webkit-transform"] =
      //   "translate3d(0," + (8 - (ddVal - minD) * 2) + "em,0)";
      // date_dd.setAttribute("top", 8 - (ddVal - minD) * 2 + "em");
      //} else {
      //	return;
      //}
    },
    //求月份最大天数
    calcDays: function(year, month) {
      var that = this;
      if (month == 1) {
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
        if (month == 3 || month == 5 || month == 8 || month == 10) {
          return 30;
        } else {
          return 31;
        }
      }
    },
		//触摸开始
    gearTouchStart:function(event) {
      console.log('触摸开始');
      let finger = event.changedTouches[0];
      this.finger.startY = finger.pageY;
      this.finger.startTime = event.timestamp || Date.now();
      //this.finger.transformY = this.$refs.list.getAttribute("scroll");
      event.preventDefault();
		},
		//手指移动
    gearTouchMove:function(event) {
      console.log('手指移动');
			var finger = event.changedTouches[0];
      this.finger.lastY = finger.pageY;
      this.finger.lastTime = event.timestamp || Date.now();
      /* 设置css */
      let move = this.finger.lastY - this.finger.startY;
      //console.log(move)
      var fontS=parseInt(window.getComputedStyle(this.$refs.date_ctrl_div).fontSize);      
      this.setStyle(move);
      console.log(this.top_yy);
      event.preventDefault();
		},
		//离开屏幕
    gearTouchEnd:function(event) {
      console.log('离开屏幕');
      let finger = event.changedTouches[0];
      this.finger.lastY = finger.pageY;
      this.finger.lastTime = event.timestamp || Date.now();

      let move = this.finger.lastY - this.finger.startY;
      /* 计算速度 */
      /* 速度计算说明
         * 当时间小于300毫秒 最后的移动距离等于 move + 减速运动距离
         * */
      
      let time = this.finger.lastTime - this.finger.startTime;
      console.log(time)
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
      //传值给父组件
      //this.$emit('valueChange', id);
		},
		setStyle:function(move, type, time){
			var fontS=parseInt(window.getComputedStyle(this.$refs.date_ctrl_div).fontSize);
			move=Math.ceil(move/fontS);
      var maxHeight=(this.maxY-this.minY);
      var top_yy=Math.ceil((this.top_yy).replace('em',''));
      if(type=='end'){
        if(Math.abs(top_yy)>=maxHeight){
          if(top_yy>0){
            this.top_yy=maxHeight+'em';
          }else{
            this.top_yy=-maxHeight+'em';
          }
          
        }else{
          if(top_yy%2!=0){
            top_yy=top_yy+1;
            this.top_yy=top_yy+'em';
          }
        }
        
      }else{
        if(top_yy>-maxHeight&&top_yy<maxHeight){
          this.top_yy=(move+top_yy)+'em';
        }else if(top_yy<-maxHeight){
          this.top_yy=-maxHeight+'em';
        }else if(top_yy>maxHeight){
          this.top_yy=maxHeight+'em';
        }else if(top_yy==-maxHeight){
          if((move+top_yy)>-maxHeight){
            this.top_yy=(move+top_yy)+'em';
          }else{
            this.top_yy=-maxHeight+'em';
          }
        }else if(top_yy==maxHeight){
          if((move+top_yy)<maxHeight){
            this.top_yy=(move+top_yy)+'em';
          }else{
            this.top_yy=maxHeight+'em';
          }
        }
      }
      
    },
    //缓速运动
    rollGear:function(move,time=1000){
      
    }
		
  }
};
</script>


