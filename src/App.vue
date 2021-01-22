<template>
  <div id="app">
    <todos-header></todos-header>
    <todos-input @release="handleRelease" @selectAll="handleSelectAll" :todos="todos"></todos-input>
    <todos-item :todos="todos" :tabTxt="tabTxt" @deleteItem="handleDelete" @storageUpdate="handleUpdate"></todos-item>
    <todos-tab :todos="todos" v-if="todos.length" @tabChange="handleTabChange" @removeCompleted="handleRemoveCompleted"></todos-tab>
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
    
  },
  methods:{
    handleRelease(text){
      this.num++;
      const obj = {
        id:this.num,
        title:text,
        completed:false
      }
      this.todos.push(obj);
      // console.log([obj]);
      // local storage 中只能存储字符串，所以要先转字符串
      let myTodos = localStorage.getItem('myTodos');
      // console.log(myTodos);
      if(myTodos){
        let todosArr = JSON.parse(myTodos);
        todosArr.push(obj)
        // console.log(JSON.parse(myTodos));
        localStorage.setItem('myTodos',JSON.stringify(todosArr));
      }else{
        localStorage.setItem('myTodos',JSON.stringify([obj]));
      }
    },
    handleDelete(id){
      // 利用id找出这一项在原数组中对应的index，然后进行删除
      const index = this.todos.findIndex(element => element.id == id);
      // console.log(index);
      this.todos.splice(index,1);
      this.num--;
      //每次删除一项数据，就重新更新item中的id
      for(let i=0;i<this.todos.length;i++){
        this.todos[i].id = i+1;
      }
      //然后更新local storage
      localStorage.setItem('myTodos',JSON.stringify(this.todos));
    },
    handleTabChange(tab){
      this.tabTxt = tab;
    },
    handleRemoveCompleted(){
      this.todos = this.todos.filter( element => !element.completed );
      //每次删除一项数据，就重新更新item中的id
      for(let i=0;i<this.todos.length;i++){
        this.todos[i].id = i+1;
      }
       //然后更新local storage
      localStorage.setItem('myTodos',JSON.stringify(this.todos));
    },
    handleSelectAll(){
      const length = this.todos.filter( e => !e.completed).length;
      if(length){
        this.todos.forEach( e => e.completed = true);
         //然后更新local storage
        localStorage.setItem('myTodos',JSON.stringify(this.todos));
      }else{
        this.todos.forEach( e => e.completed = false);
         //然后更新local storage
        localStorage.setItem('myTodos',JSON.stringify(this.todos));
      }
    },
    handleUpdate(id){
       //然后更新local storage
      //  console.log(111);
      //因为在点击复选框时，它先触发自定义事件，然后再改变自身的状态，所以在这里我们拿到点击的那一项，直接对它的completed值进行取反，算是提前改变了
        const index = this.todos.findIndex(element => element.id == id);
        const beforeArr =  JSON.parse(localStorage.getItem('myTodos'));
        for(let i=0;i<beforeArr.length;i++){
          if(i == index){
            beforeArr[i].completed = !beforeArr[i].completed;
          }
        }
        localStorage.setItem('myTodos',JSON.stringify(beforeArr));
    }
    
  },
  created(){
    // 每次重新打开页面的时候就读取本地存储，然后更新todos数据
    //由于每次重新打开，this.num为0，如果之前有数据，那么再添加事项会造成id重复的情况
    //所以在读取更新todos的时候，也要更新this.num
    let myTodos = localStorage.getItem('myTodos');
    // console.log(myTodos);
    if(myTodos){
      this.todos = JSON.parse(myTodos);
      // console.log(this.todos[this.todos.length - 1].id);
      this.num = this.todos[this.todos.length - 1].id;  //更新用于记录id的num,避免id重复的情况
    }else{
      return;
    }
  },
  data(){
    return {
      todos:[],
      num:0,
      tabTxt:'All'
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
