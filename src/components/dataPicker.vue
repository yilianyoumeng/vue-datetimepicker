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
          <data-pickerItem itemUnit='年' type="yy" @getVal="getYear" @getTopD="getTopD" :month="month" :day="day"></data-pickerItem>
          <data-pickerItem itemUnit='月' type="mm" @getVal="getMonth" @getTopD="getTopD" :year="year" :day="day"></data-pickerItem>
          <data-pickerItem itemUnit='日' type="dd" @getVal="getDay" :year="year" :month="month" :topValD="topValD" :addTopD="addTopD" :timestamp="timestamp"></data-pickerItem>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
import dataPickerItem from './dataPickerItem.vue';
export default {
  components: {
    dataPickerItem
  },
  data() {
    return {
      year:0,
      month:-1,
      day:0,
      topValD:0,
      addTopD:0,
      timestamp:0
    };
  },
  props: {
    minY: {
      default:2000
    },
    minM: {
      default:1
    },
    minD: {
      default:1
    },
    maxY: {
      default:2018
    },
    maxM: {
      default:12
    },
    maxD: {
      default:31
    },
  },
  methods: {
    getYear:function(val){
      this.year=val;
    },
    getMonth:function(val){
      this.month=val;
    },
    getDay:function(val){
      this.day=val;
    },
    getTopD:function(val){
      if(val!=undefined){
        this.topValD=val[0];
        this.addTopD=val[1];
        this.timestamp=val[2];
      }
      
    }
  }
};
</script>


