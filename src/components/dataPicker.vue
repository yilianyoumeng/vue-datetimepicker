<template>
	<div>
		<!--年月日-->
		<div class="date_ctrl slideInUp">
			<div class="date_btn_box">
				<div class="date_btn lcalendar_cancel">取消</div>
				<div class="date_btn lcalendar_finish">确定</div>
			</div>
			<div class="date_roll_mask">
				<div class="date_roll">
					<div>
						<div class="gear date_yy" data-datetype="date_yy" :val="dateArr.yy" :top="top_yy" :style="'-webkit-transform :translate3d(0,' + top_yy + ',0)'">
							<div class='tooth'>that.minY</div>
							<div class='tooth' v-for="p in (passY-1)" >that.minY + p</div>
						</div>
						<div class="date_grid">
							<div>年</div>
						</div>
					</div>
					<div>
						<div class="gear date_mm" data-datetype="date_mm" :val="dateArr.mm" :top="top_mm"></div>
						<div class="date_grid">
							<div>月</div>
						</div>
					</div>
					<div>
						<div class="gear date_dd" data-datetype="date_dd" :val="dateArr.dd" :top="top_mm"></div>
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
export default {
  data() {
    return {
    	minY : 1900,
		minM : 1,
		minD : 1,
		maxY : 2099,
		maxM : 12,
		maxD : 31,
		passY : 0,
		top_yy:'',
		top_mm:'',
		top_dd:''
    };
  },
  props: {
    // isSelectItem: {
    //   default: false
    // },
    // placeholder: {
    //   default: ""
    // },
    dateArr: {
      default: function() {
        return {};
      }
    }
  },
  mounted() {
	  this.dateTimeCtrlInit()
  },
  methods: {
    dateTimeCtrlInit:function () {
		var that=this;
		var date = new Date();
		that.dateArr = {
			yy: date.getYear(),
			mm: date.getMonth(),
			dd: date.getDate() - 1,
			hh: date.getHours(),
			mi: date.getMinutes()
		};
		// if (/^\d{4}-\d{1,2}-\d{1,2}\s\d{2}:\d{2}$/.test(that.trigger.value)) {
		// 	rs = that.trigger.value.match(/(^|-|\s|:)\d{1,4}/g);
		// 	that.dateArr.yy = rs[0] - that.minY;
		// 	that.dateArr.mm = rs[1].replace(/-/g, "") - 1;
		// 	that.dateArr.dd = rs[2].replace(/-/g, "") - 1;
		// 	that.dateArr.hh = parseInt(rs[3].replace(/\s0?/g, ""));
		// 	that.dateArr.mi = parseInt(rs[4].replace(/:0?/g, ""));
		// } else {
		// 	that.dateArr.yy = that.dateArr.yy + 1900 - that.minY;
		// }
		// that.gearDate.querySelector(".date_yy").setAttribute("val", dateArr.yy);
		// that.gearDate.querySelector(".date_mm").setAttribute("val", dateArr.mm);
		// that.gearDate.querySelector(".date_dd").setAttribute("val", dateArr.dd);
		this.setDateGearTooth();
		// that.gearDate.querySelector(".time_hh").setAttribute("val", dateArr.hh);
		// that.gearDate.querySelector(".time_mm").setAttribute("val", dateArr.mi);
		// setTimeGearTooth();
	},
	setDateGearTooth:function () {
		var that = this;
		that.passY = that.maxY - that.minY + 1;
		//var date_yy = _self.gearDate.querySelector(".date_yy");
		var itemStr = "";
		//if (date_yy && date_yy.getAttribute("val")) {
			//得到年份的值
			var yyVal = parseInt(that.dateArr.yy);
			//p 当前节点前后需要展示的节点个数
			// for (var p = 0; p <= passY - 1; p++) {
			// 	itemStr += "<div class='tooth'>" + (that.minY + p) + "</div>";
			// }
			// date_yy.innerHTML = itemStr;
			var top = Math.floor(parseFloat(that.top_yy));
			if (!isNaN(top)) {
				top % 2 == 0 ? (top = top) : (top = top + 1);
				top > 8 && (top = 8);
				var minTop = 8 - (that.passY - 1) * 2;
				top < that.minTop && (top = that.minTop);
				//that.date_yy.style["-webkit-transform"] = 'translate3d(0,' + top + 'em,0)';
				that.top_yy=top+ 'em';
				//that.date_yy.setAttribute('top', top + 'em');
				
				yyVal = Math.abs(top - 8) / 2;
				//that.date_yy.setAttribute("val", yyVal);
				that.dateArr.yy=yyVal;
			} else {
				that.top_yy=(8 - yyVal * 2)+'em'
				//date_yy.style["-webkit-transform"] = 'translate3d(0,' + (8 - yyVal * 2) + 'em,0)';
				// date_yy.setAttribute('top', 8 - yyVal * 2 + 'em');
			}
		//} else {
		//	return;
		//}
		//var date_mm = that.gearDate.querySelector(".date_mm");
		//if (date_mm && date_mm.getAttribute("val")) {
			itemStr = "";
			//得到月份的值
			var mmVal = parseInt(that.dateArr.mm);
			var maxM = 11;
			var minM = 0;
			//当年份到达最大值
			if (yyVal == passY - 1) {
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
			date_mm.innerHTML = itemStr;
			if (mmVal > maxM) {
				mmVal = maxM;
				date_mm.setAttribute("val", mmVal);
			} else if (mmVal < minM) {
				mmVal = maxM;
				date_mm.setAttribute("val", mmVal);
			}
			date_mm.style["-webkit-transform"] = 'translate3d(0,' + (8 - (mmVal - minM) * 2) + 'em,0)';
			date_mm.setAttribute('top', 8 - (mmVal - minM) * 2 + 'em');
		//} else {
		//	return;
		//}
		//var date_dd = that.gearDate.querySelector(".date_dd");
		//if (date_dd && date_dd.getAttribute("val")) {
			itemStr = "";
			//得到日期的值
			var ddVal = parseInt(date_dd.getAttribute("val"));
			//返回月份的天数
			var maxMonthDays = calcDays(yyVal, mmVal);
			//p 当前节点前后需要展示的节点个数
			var maxD = maxMonthDays - 1;
			var minD = 0;
			//当年份月份到达最大值
			if (yyVal == passY - 1 && that.maxM == mmVal + 1) {
				maxD = that.maxD - 1;
			}
			//当年、月到达最小值
			if (yyVal == 0 && that.minM == mmVal + 1) {
				minD = that.minD - 1;
			}
			for (var p = 0; p < maxD - minD + 1; p++) {
				itemStr += "<div class='tooth'>" + (minD + p + 1) + "</div>";
			}
			date_dd.innerHTML = itemStr;
			if (ddVal > maxD) {
				ddVal = maxD;
				date_dd.setAttribute("val", ddVal);
			} else if (ddVal < minD) {
				ddVal = minD;
				date_dd.setAttribute("val", ddVal);
			}
			date_dd.style["-webkit-transform"] = 'translate3d(0,' + (8 - (ddVal - minD) * 2) + 'em,0)';
			date_dd.setAttribute('top', 8 - (ddVal - minD) * 2 + 'em');
		//} else {
		//	return;
		//}
	}
  }
};
</script>
<style lang='less' scoped>
@import "../assets.less";
.selectNormal {
  display: inline-block;

  position: relative;
  .selectParent {
    .placeholder {
      color: #c7c7cd;
    }
    .jiao {
      .afterJiao;
    }
  }
  .bg {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 999;
    text-align: center;
    &:after {
      content: "";
      display: inline-block;
      vertical-align: middle;
      height: 100%;
      width: 0;
    }
    .selectItemBox {
      text-align: left;
      background-color: @white;
      // text-align: center;
      width: 684*@px1;
      // height: 464*@px1;
      border-radius: 4*@px1;
      display: inline-block;
      vertical-align: middle;
      margin: auto;
      .selectItem {
        font-size: 24*@px1;
        color: @color333;
        border-bottom: 1px solid @colorddd;
        line-height: 2;
        padding: 20*@px1;
        &:last-of-type {
          border: none;
        }
        &:first-of-type {
          border-top: none;
        }
      }
    }
  }
}
</style>


