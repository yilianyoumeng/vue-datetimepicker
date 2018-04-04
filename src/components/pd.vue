<template>
  <div class="formBox">
    <div class="input_box" >
      <div class="label_div">选择原因</div>
      <input type="text" v-model="pickerVal">
      <data-picker @getVal='getVal'></data-picker>
    </div>
    
  </div>
</template>
<script>
  import dataPicker from './dataPicker.vue'
  export default{
    name: 'pd',
    data () {
      return {
        pickerVal:''
      }
    },
    components: {
      dataPicker
    },
    methods:{
      getVal:function(val){
        console.log('点击确定');
        console.log(val);
        this.pickerVal=val.year+'-'+(val.month+1)+'-'+val.day;
      }
    }
  }
</script>
<style lang="less" scoped>
@import '../assets.less';
.formBox{
  background-color: #fff;

  .input_box{
    width: 668*@px1;
    margin:auto;
    background: #fff;
    font-size: 0;
    .label_div{
      color: #666;
      width: 200*@px1;
      display: inline-block;
      font-size: 36*@px1;
      line-height: 3;
    }
    .selectNormal{
      width:6rem;
      font-size: 36*@px1;
    }
  }
}
</style>

