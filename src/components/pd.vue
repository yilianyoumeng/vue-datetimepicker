<template>
  <div>
    <div>
      <div>{{pickerVal}}</div>
      <data-picker v-model="pickerVal" inpClass="inp" type="onlyYM" ></data-picker>
    </div>
  </div>
</template>
<script>
  import dataPicker from './dataPicker.vue'
  export default{
    name: 'pd',
    data () {
      return {
        pickerVal:'',
        //maxDate:'2020-05-10'//自定义最大时间
      }
    },
    components: {
      dataPicker
    },
    methods:{
      
    }
  }
</script>
<style lang="less">
  .inp{border:1px solid #ddd;}
</style>

