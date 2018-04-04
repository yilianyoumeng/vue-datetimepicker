<template>
	<div class="bg">
		<!--年月日-->
		<div class="date_ctrl slideInUp">
			<div class="date_btn_box">
				<div class="date_btn lcalendar_cancel">取消</div>
				<div class="date_btn lcalendar_finish" @click="yesFn">确定</div>
			</div>
			<div class="date_roll_mask">
				<div class="date_roll">
          <data-pickerItem itemUnit='年' type="yy" @getVal="getYear" @getTopD="getTopD" @getTopM="getTopM" :month="month" :day="day"></data-pickerItem>
          <data-pickerItem itemUnit='月' type="mm" @getVal="getMonth" @getTopD="getTopD" :year="year" :day="day" :topValM="topValM" :addTopM="addTopM" :timestampM="timestampM"></data-pickerItem>
          <data-pickerItem itemUnit='日' type="dd" @getVal="getDay" :year="year" :month="month" :topValD="topValD" :addTopD="addTopD" :timestampD="timestampD"></data-pickerItem>
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
      timestampD:0,
      topValM:0,
      addTopM:0,
      timestampM:0
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
        this.timestampD=val[2];
      }
      
    },
    getTopM:function(val){
      if(val!=undefined){
        this.topValM=val[0];
        this.addTopM=val[1];
        this.timestampM=val[2];
      }
      
    },
    //点击确定
    yesFn:function(){
      if(this.year==0){
        this.year=new Date().getFullYear();
      }
      if(this.month==-1){
        this.month=new Date().getMonth();
      }
      if(this.day==0){
        this.day=new Date().getDate();
      }
      this.$emit('getVal',{year:this.year,month:this.month,day:this.day})
    }
  }
};
</script>


