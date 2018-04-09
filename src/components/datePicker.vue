<template>
<div class="datePickerBox">
  <input type="text" :class="inpClass" v-model="pickerVal" @click="isPickerShow=true">
	<div class="bg" v-if="isPickerShow">
		<!--年月日-->
		<div class="date_ctrl slideInUp">
			<div class="date_btn_box">
				<div class="date_btn lcalendar_cancel">取消</div>
				<div class="date_btn lcalendar_finish" @click="yesFn">确定</div>
			</div>
			<div class="date_roll_mask">
				<div :class="this.type+' date_roll'">
          <date-pickerItem v-if="isShowY" itemUnit='年' type="yy" :maxDate="maxDateObj" :minDate="minDateObj" @getVal="getYear" @getTopD="getTopD" @getTopM="getTopM" :month="month" :day="day"></date-pickerItem>
          <date-pickerItem v-if="isShowM" itemUnit='月' type="mm" :maxDate="maxDateObj" :minDate="minDateObj" @getVal="getMonth" @getTopD="getTopD" :year="year" :day="day" :topValM="topValM" :addTopM="addTopM" :timestampM="timestampM"></date-pickerItem>
          <date-pickerItem v-if="isShowD" itemUnit='日' type="dd" :maxDate="maxDateObj" :minDate="minDateObj" @getVal="getDay" :year="year" :month="month" :topValD="topValD" :addTopD="addTopD" :timestampD="timestampD"></date-pickerItem>
          <date-pickerItem v-if="isShowH" itemUnit='时' type="hh" :maxDate="maxDateObj" :minDate="minDateObj" @getVal="getHour" :topValD="topValD" :addTopD="addTopD"></date-pickerItem>
          <date-pickerItem v-if="isShowMi" itemUnit='分' type="mi" :maxDate="maxDateObj" :minDate="minDateObj" @getVal="getSecond" :topValD="topValD" :addTopD="addTopD"></date-pickerItem>
				</div>
			</div>
		</div>
	</div>
</div>
</template>
<script>
import datePickerItem from './datePickerItem.vue';
export default {
  components: {
    datePickerItem
  },
  data() {
    return {
      pickerVal:'',
      isPickerShow:false,
      year:0,
      month:-1,
      day:0,
      hour:0,
      second:0,
      topValD:0,
      addTopD:0,
      timestampD:0,//时间戳
      topValM:0,
      addTopM:0,
      timestampM:0,
      maxDateObj:{},
      minDateObj:{},
      isShowY:false,
      isShowM:false,
      isShowD:false,
      isShowH:false,
      isShowMi:false
    };
  },
  props: {
    maxDate:{
      default:'2025-12-31'
    },
    minDate:{
      default:'2016-01-1'
    },
    inpClass:{
      default:''
    },
    type:{
      default:'datepicker'
    } 
  },
  mounted: function () {
    var maxDateArr=this.maxDate.split('-');
    var minDateArr=this.minDate.split('-');
    if(this.type=="onlyYM"){//只有年月
      this.maxDateObj.yy=parseInt(maxDateArr[0]);
      this.maxDateObj.mm=parseInt(maxDateArr[1])-1;
      this.maxDateObj.dd=0;

      this.minDateObj.yy=parseInt(minDateArr[0]);
      this.minDateObj.mm=parseInt(minDateArr[1])-1;
      this.minDateObj.dd=0;
    }else if(this.type=="datepicker"){//年月日
      this.maxDateObj.yy=parseInt(maxDateArr[0]);
      this.maxDateObj.mm=parseInt(maxDateArr[1])-1;
      this.maxDateObj.dd=parseInt(maxDateArr[2]);

      this.minDateObj.yy=parseInt(minDateArr[0]);
      this.minDateObj.mm=parseInt(minDateArr[1])-1;
      this.minDateObj.dd=parseInt(minDateArr[2]);
    }else if(this.type=="datetimePicker"){//年月日时分
      this.maxDateObj.yy=parseInt(maxDateArr[0]);
      this.maxDateObj.mm=parseInt(maxDateArr[1])-1;
      this.maxDateObj.dd=parseInt(maxDateArr[2]);
      this.maxDateObj.hh=parseInt(maxDateArr[3]);
      this.maxDateObj.mi=parseInt(maxDateArr[4]);

      this.minDateObj.yy=parseInt(minDateArr[0]);
      this.minDateObj.mm=parseInt(minDateArr[1])-1;
      this.minDateObj.dd=parseInt(minDateArr[2]);
      this.minDateObj.hh=parseInt(minDateArr[3]);
      this.minDateObj.mi=parseInt(minDateArr[4]);
    }else if(this.type=="timePicker"){
      this.maxDateObj.hh=parseInt(maxDateArr[3]);
      this.maxDateObj.mi=parseInt(maxDateArr[4]);

      this.minDateObj.hh=parseInt(minDateArr[3]);
      this.minDateObj.mi=parseInt(minDateArr[4]);
    }else{
      alert('初始化请设置正确的日期格式');
    }
    //控件类型
    if(this.type=="datepicker"){
      this.isShowY=true;
      this.isShowM=true;
      this.isShowD=true;
      this.isShowH=false;
      this.isShowMi=false;
    }else if(this.type=="timePicker"){
      this.isShowY=false;
      this.isShowM=false;
      this.isShowD=false;
      this.isShowH=true;
      this.isShowMi=true;
    }else if(this.type=="datetimePicker"){
      this.isShowY=true;
      this.isShowM=true;
      this.isShowD=true;
      this.isShowH=true;
      this.isShowMi=true;
    }else if(this.type=="onlyYM"){
      this.isShowY=true;
      this.isShowM=true;
      this.isShowD=false;
      this.isShowH=false;
      this.isShowMi=false;
    }else{
       alert('初始化请设置正确的type');
    }
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
    getHour:function(val){
      this.hour=val;
    },
    getSecond:function(val){
      this.second=val;
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
      if(this.hour==0){
        this.hour= new Date().getHours();
      }
      if(this.second==0){
        this.second= new Date().getMinutes();
      }
      
      if(this.type=="datepicker"){
        this.pickerVal=this.year+'-'+(this.month+1)+'-'+this.day;
        
      }else if(this.type=="timePicker"){
        this.pickerVal=this.hour+'-'+this.second
      }else if(this.type=="datetimePicker"){
        this.pickerVal=this.year+'-'+(this.month+1)+'-'+this.day+' '+this.hour+':'+this.second
      }else if(this.type=="onlyYM"){
        this.pickerVal=this.year+'-'+(this.month+1)
      }
      this.$emit('input',this.pickerVal);
      this.isPickerShow=false;
    }
  }
};
</script>


