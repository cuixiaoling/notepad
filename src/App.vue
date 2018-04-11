<template>
  <div id="app">
    <div  class="wrapper">
      <h1 class="line1"> 记事本</h1>
      <div class="input"> 
        <input type="" name="" class="left" placeholder="待办事项" v-model="inputValue">
        <span class="right" @click="subBtnFun"> 提交</span>
      </div>
      <div class="content">
        <div class="weiwancehg">
          <div class="title" @click="noDoneClick">未完成</div>
          <div v-for="(item,index) in noDoneList" class="line" v-if="noDoneShow &&item.type ==1 " @click="moveToDone(item.id)">
            <span class="line-icon" style=""></span>
            <span style="margin-left: 5px;">{{item.content}}</span>
            <span class="line-quxiao">完成</span>
          </div>
        </div>
        <div class="yiwancheng">
          <div class="title">已完成</div>
          <div v-for="(item,index) in noDoneList" class="line" @click="delFun(item.id)" v-if=" item.type ==2">
            <span class="line-icon" style=""></span>
            <span style="margin-left: 5px;">{{item.content}}</span>
            <span>{{item.time}}</span>
            <span class="line-quxiao">删除</span>
          </div>
        </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {  mapState } from "vuex";
export default {
  name: 'App',
  data(){
    return{
      noDoneShow:false,
      inputValue:'',
      noDoneList:[],
      // doneList:[],
      }
  },
  computed:{
    ...mapState([
      'event'
    ])
  },
  components: {
    // HelloWorld
  },
  methods:{
    noDoneClick(){
      this.noDoneShow = this.noDoneShow ? false: true
    },
    // 提交事件
    subBtnFun(){
      // if(!this.inputValue) return;
      // this.noDoneList.unshift(this.inputValue);
      // this.noDoneShow = true;
      // this.inputValue ='';
      // 函数的参数值
      let params = {
        id:0,
        type:1,
        content:'',
        time:''
      }
      // let params =1;
      this.inputValue = this.inputValue.trim();
      if(this.inputValue){
        params.content = this.inputValue;
        this.$store.dispatch('addevent',params);
        this.noDoneShow = true;
        this.noDoneList = this.event;
        this.inputValue ='';
      }

    },
    // 点击完成事件
    moveToDone(id){
      console.log(id,'xoxoxo')
      // console.log(this.noDoneList[index])
      // this.doneList.unshift(this.noDoneList[index])
      // this.noDoneList.splice(index,1);
      this.$store.dispatch('eventdone',id);
    },
    // 删除事件
    delFun(id){
      // this.doneList.splice(index,1);
      this.$store.dispatch('deleteEvent', id)
    }
  },
  created(){
    console.log(this.noDoneShow );
    this.noDoneList = this.event;
    if(this.noDoneList) this.noDoneShow = true;

  }
}
</script>

<style>
html,body{
  padding: 0;
  margin: 0;
  width: 100%;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.wrapper{

}
.line1{
  background: #1facb4;
  color: #fff;
  margin: 0;
  padding: 10px;
  padding-left: 0;
  padding-right: 0;
}
.input{
  /*width: 100%;*/
  padding: 20px;
  /*box-sizing: border-box;*/
  display: flex;
  justify-content: space-between;
  height: 35px; 
  line-height: 35px;
}
.left{
 width: 70%;
 padding: 5px;
}
.right{
  width: 20%;
  background:#1facb4;
  color: #fff;
  border-radius: 4px;
}
.content{
  text-align: left;
  padding: 15px;
  line-height: 35px;
}
.content .title{
  color: #fff;
  background: #1facb4;
  margin-bottom: 2px;
  padding-left: 10px;
}
.line{
  border-bottom: 0.5px solid rgb(236, 236, 236);
}
.line-icon{
  display: inline-block; 
  width: 18px; 
  height: 18px; 
  background: transparent; 
  vertical-align:middle; 
  border: 1px solid #ccc;
}
.line-quxiao{
  border: 1px solid #ccc; 
  border-radius: 4px; 
  display: inline-block; 
  padding: 1px 10px; 
  line-height: 20px; 
  float: right; 
  margin-top: 5px;
}
</style>
