<template>
  <div id="app">
    <todos-header/>
    <todos-input @addEvent="handleAdd"  @allSelect="handleSelect"/>
    <div v-for="(item,index) in filterThings" :key="item">
      <todos-item :todos="item" :num="index" @handleRemove="handleDelete"></todos-item>
    </div>
    <todos-tab v-show="this.things.length"  :items="things" @replace="handleReplace" @itemChange="handleChange"/>
  </div>
</template>

<script>
import TodosHeader from "./components/Header.vue";
import TodosInput from "@/components/TodosInput.vue";
import TodosItem from "./components/TodosItem.vue";
import TodosTab from "./components/TodosTab.vue";

export default {
  name: 'App',
  components: {
    TodosHeader,
    TodosInput,
    TodosItem,
    TodosTab
  },
  computed:{
    filterThings(){
      if(this.actived == "All"){
        return this.things;
      }else if(this.actived == "Active"){
        return this.things.filter(item => !item.completed);
      }else{
        return this.things.filter(item => item.completed);
      }
    }
  },
  methods:{
    handleAdd(value){
      this.count++;
      const obj = {
        id: this.count,
        title: value,
        completed: false
      }
      this.things.push(obj);
    },
    handleDelete(num){
      // console.log(num);
      this.things.splice(num,1);
      for(let i = 0;i<this.things.length;i++){
        this.things[i].id = i + 1;
      }
      this.count--;
    },
    handleReplace(){
      this.things = this.things.filter( item => !item.completed);
    },
    handleChange(text){
      // console.log(111);
      this.actived = text;
    },
    handleSelect(flag){
      this.things.forEach(element => {
          element.completed = flag;
      });
    }
  },
  data(){
    return {
      things:[
        // {
        //   id:5,
        //   title:'搬砖',
        //   completed:false
        // }
      ],
      count:0,
      actived:'All'
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
   box-sizing: border-box;
}
body,html{
  background: #f5f5f5;
}
#app {
  position: relative;
  width: 550px;
  min-width: 350px;
  margin: 150px auto;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
}
</style>
