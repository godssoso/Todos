<!-- todos的输入框部分 -->
<template>
<div class='inp'>
    <!-- 监听回车键，发布代做事项 -->
    <input type="text" placeholder="What need to be done?" v-model="val" class="new-todo" @keyup.enter="release">
    <div class="toggle-all"></div>
    <label for="toggle-all" @click="selectAll" :class="{show:!todos.filter(e => !e.completed).length}" v-if="todos.length"></label>
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
    }
},
data() {
//这里存放数据
return {
    val:''
};
},
//监听属性 类似于data概念
computed: {},
//监控data中的数据变化
watch: {},
//方法集合
methods: {
   release(){
       if(this.val){
           //先发布事件，将数据传给父组件，再将输入框清空
           this.$emit('release',this.val); 
           this.val = '';
       }
   },
   selectAll(){
       this.$emit('selectAll')
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
   .inp{
       position: relative;
   }
   .new-todo{
        padding: 16px 16px 16px 60px;
        border: none;
        background: rgba(0, 0, 0, 0.003);
        box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
        margin: 0;
        width: 100%;
        font-size: 24px;
        font-family: inherit;
        font-weight: inherit;
        line-height: 1.4em;
        outline: 0;
        border-bottom: 1px solid #ddd;
    }
    .toggle-all{
        position: absolute;
        width: 45px;
        height: 65px;
        /* background: red; */
        left: 0;
        top: 0;
        
    }
    label{
        width: 60px;
        height: 34px;
        font-size: 0;
        position: absolute;
        top: 13px;
        left: -4px;
        transform: rotate(90deg);
        cursor: pointer;
    }
    label::before{
        content: '❯';
        font-size: 22px;
        color: #e6e6e6;
        padding: 10px 27px 10px 27px;
        cursor: pointer;
    }
    label.show::before{
        color: black;
    }
</style>