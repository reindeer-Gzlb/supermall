<template>
  <div class="tab-control">
    <div class="tab-control-item" 
    v-for="(item,index) in title" :key="index"
     :class="{active: index === counter}" @click="itemclick(index)">
      <span>{{item}}</span>
    </div>
  </div> 
</template>
 
<script>
export default {
  name:'TabControl',
  props:{
    title:{
      type:Array,
      default(){
        return []
      }
    }
  },
  data(){
    return{
      counter:0,
    }
  },
  methods:{
    itemclick(index){
      this.counter = index;
      this.$emit('tabClick',index);
    }
  }
}
</script>

<style>
.tab-control{
  display: flex;
  line-height: 40px;
  height: 40px;
  text-align: center;
  background-color: #fff;
}
.tab-control-item{
  flex: 1;
  
}
.tab-control-item span{
  padding: 5px;
}
.active{
  color:var(--color-high-text);
}
.active span{
  border-bottom:  3px solid var(--color-high-text);
}
</style>