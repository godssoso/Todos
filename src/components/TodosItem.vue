<!-- todos的具体项 -->
<template>
<div class='todo-items'>
    <div class="item-left">
        <!-- checkbox类input标签用v-model指令绑定状态 -->
        <input type="checkbox" name="" id="" @click="handleClick(todos)" v-model="todos.completed">
    </div>
    <div class="item-title"><span :class="{done:todos.completed}">{{ todos.title }}</span></div>
    <div class="item-right" @click="remove(num)">×</div>
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
        type:Object,
        default:[]
   },
   num:{
       type:Number,
        default:0
   }
},
data() {
//这里存放数据
return {

};
},
//监听属性 类似于data概念
computed: {},
//监控data中的数据变化
watch: {},
//方法集合
methods: {
    handleClick(todos){
        todos.completed = !todos.completed;
        console.log(todos.completed);
    },
    remove(num){
        // console.log(num);
        this.$emit('handleRemove',num);
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
</style>