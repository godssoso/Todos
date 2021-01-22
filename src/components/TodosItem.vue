<!-- todos的具体项 -->
<template>
<div>
    <div class='todo-items' v-for="(item,index) in showTodos" :key="item.id">
        <div class="item-left">
            <!-- checkbox类input标签用v-model指令绑定状态 -->
            <input type="checkbox" v-model="item.completed" name="" id="" @click="storageUpdate(item.id)">
        </div>
        <div :class="['item-title',{done:item.completed}]">
            <span @dblclick="reedit(index)" :class="{none:flag && index == currentIndex}">{{ item.title }}</span>
            <input type="text" v-focus  v-model="item.title" :class="{none:(!flag) || index != currentIndex}" @blur="finish(item)" @keyup.enter="finish(item)">
        </div>
        <!-- 删除项不能直接传index进行删除，因为此时的index是在过滤属性中的index，而非原数组中的index,所以要传入一个不变项id -->
        <div :class="['item-right',{hidden:flag}]" @click="deleteItem(item.id)">X</div>
    </div>
</div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
//import引入的组件需要注入到对象中才能使用
components: {},
props:{
   todos:{
       type:Array,
       default:function(){
           return []
       }
   },
   tabTxt:{
       type:String,
       default:'All'
   }
},
data() {
//这里存放数据
return {
    flag:false,
    currentIndex:NaN
};
},
//监听属性 类似于data概念
computed: {
    //计算属性，用于底部三个tab选项的切换
    showTodos(){
        if(this.tabTxt === "All"){
            return this.todos
        }else if(this.tabTxt === "Active"){
            return this.todos.filter( element => !element.completed)
        }else{
            return this.todos.filter( element => element.completed)
        }
    }
},
//监控data中的数据变化
watch: {},
//自定义指令  注意点，什么时候添加insert钩子函数  v-focus让双击显示出的input框自动聚焦
directives: {
    focus:function (el) {
        el.focus();
    }
},
//方法集合
methods: {
    //删除某一项
   deleteItem(id){
       this.$emit('deleteItem',id)
   },
   //点击复选框时，更新本地存储中记录的状态
   storageUpdate(id){
      this.$emit('storageUpdate',id);
   },
    //直接双击item项进行重新编辑
   reedit(index){
       this.currentIndex = index;
       this.flag = !this.flag;
   },
  // 编辑完成input框失去焦点后更新数据
  // 新增监听enter键
   finish(item){
      //console.log(111);
       this.currentIndex = NaN;
       this.flag = !this.flag;
      //失去焦点后，还要重新更新local storage中的内容
      // 如果此时内容为空，责默认删除该项
        if(!item.title){
            this.$emit('deleteItem',item.id);
        }else{
            this.$emit('reedit')
        }
   }
},
//生命周期 - 创建完成（可以访问当前this实例）
created() {

},
//生命周期 - 挂载完成（可以访问DOM元素）
mounted() {

},
beforeCreate() {}, //生命周期 - 创建之前
beforeMount() {}, //生命周期 - 挂载之前
beforeUpdate() {}, //生命周期 - 更新之前
updated() {}, //生命周期 - 更新之后
beforeDestroy() {}, //生命周期 - 销毁之前
destroyed() {}, //生命周期 - 销毁完成
activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
}
</script>
<style  scoped>
   .todo-items{
       display: flex;
       justify-content: space-between;
       align-items: center;
       font-size: 24px;
       widows: 100%;
       height: 58px;
       padding: 0 15px;
       border-bottom: 1px solid #ddd;
       background: rgba(0, 0, 0, 0.003);
       color: #4d4d4d;
   }
   .item-left input{
       cursor: pointer;
   }
   .item-title{
       width: 430px;
       height: 57px;
       line-height: 57px;
   }
   .item-title span{
       width: 100%;
       display: inline-block;
       height: 100%;
   }
   .item-title input{
        width: 490px;
        height: 55px;
        font-size: 24px;
        color: #f00;
        border: 0;
        outline: 1px solid #f00;
        padding-left: 25px;
        background: #f8f8f8;
   }
   .item-right{
       cursor: pointer;
       font-size: 24px;
       color: rgba(175, 47, 47, 0.5);
       visibility: hidden;
   }
   .todo-items:hover .item-right{
       visibility: visible;
   }
   .done{
       color: #ccc;
       text-decoration: line-through;
   }
   .none{
       display: none!important;
   }
   .hidden{
       visibility: hidden!important;
   }
</style>