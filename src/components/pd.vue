<template>
  <div>
    <div>
      <div>{{pickerVal}}</div>
      <date-picker v-model="pickerVal" inpClass="inp" :maxDate="maxDate" :minDate="minDate" type="datepicker" ></date-picker><!--datetimePicker onlyYM datepicker timePicker-->
    </div>
  </div>
</template>
<script>
  import datePicker from './datePicker.vue'
  export default{
    name: 'pd',
    data () {
      return {
        pickerVal:'',
        maxDate:'2010-05-10',//自定义最大时间
        minDate:'1992-07-20'
      }
    },
    components: {
      datePicker
    },
    methods:{
      
    }
  }
</script>
<style lang="less">
  .inp{border:1px solid #ddd;}
</style>

