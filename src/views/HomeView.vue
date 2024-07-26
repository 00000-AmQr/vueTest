<template>
  <div class="home">
    <div class="top">
      <label class="topTitle">ToDoList</label>
      <input class="topInput" v-model="value">
      <button class="topBut" @click="add()">确定</button>
    </div>
    <div class="list">
      <h2  class="listTit">
        "正在进行"
        <span class="listCount">({{this.list.length}})</span>
      </h2>
      <ol>
        <li class="listItem" v-for="item in list" :key="item.id">
        <input type="checkbox" @click="completed(item.id)">
        <input v-bind:value="item.name" v-on:input="changeValue" :disabled="item.status" type="text" :class="[item.status?'input':'active']">
        <a href="##" @click="handleDelete(item.id)">删除</a>
        <a v-if="item.status" href="##" @click="handleEdit(item.id)">修改</a> 
        <a v-else href="##" @click="confirm(item.id)">完成</a>
      </li>
      </ol>
    </div>
    <div class="list">
      <h2  class="listTit">
        "已经完成"
        <span class="listCount">({{this.selected.length}})</span>
      </h2>
      <ol >
        <li class="listItem" v-for="newItem in selected" :key="newItem.id">
          <span>{{ newItem.name }}</span>
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  },
  data(){
    return{
      selected:[],
      currentValue:'',
      value:'',
      list:[
        {
          id:0,
          name:'aaa',
          status:true
        }
      ]
    }
  },
  methods:{
    add(){
      console.log("value:",this.value);
      var item={
        id:this.list.length,
        name:this.value,
        status:true
      }
      this.list.push(item)
      this.value='';
      console.log(this.list);
    },
    handleDelete(id){
      this.list=this.list.filter(item=>item.id!=id)
      console.log(this.list);
    },
    handleEdit(id){
      this.list.forEach(item => {
        if(item.id==id){
          item.status=false
        }
      })
    },
    changeValue(event){
      this.currentValue=event.target.value
      console.log("value",event.target.value)

    },
    confirm(id){
      this.list.forEach(item => {
        if(item.id==id){
          if(this.currentValue!=''){
            item.name=this.currentValue
          }
          item.status=true
        }
      })
      this.currentValue='';
    },
    completed(id){
      this.list.forEach(item => {
        if(item.id==id){
          var newItem={
            id:this.selected.length,
            name:item.name
          }
          this.selected.push(newItem)
        }
      })
      this.handleDelete(id);
      console.log(this.selected)
    }
  }
}
</script>

<style lang="css">
.top{
  background-color: black;height: 50px;line-height: 50px;
}
.topTitle{
  width: 100px;float: left;color: #ddd;
}
.topInput{
  width: 60%;height: 24px;border-radius: 5px;
}
.topBut{
  margin-left: 10px;
}
.list{
  width: 60%;
}
.listTit{
  display: flex;
}
.listCount{
  margin-left: 20px;
}
.listItem{
  height: 50px;line-height: 50px;display: flex;justify-content: space-between;}
.active{
  border: 1px solid;
  background-color: ghostwhite;
}
.input{
  border: 0px;background-color: white;
}
</style>
