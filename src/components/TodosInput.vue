<!-- todos的输入框部分 -->
<template>
<div class='inp'>
    <input type="text" placeholder="What need to be done?" class="new-todo" @keyup.enter="addTodos">
    <div class="toggle-all"></div>
    <label for="toggle-all" @click="selectAll" :class="{show:isshow}"></label>
</div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
//import引入的组件需要注入到对象中才能使用
components: {},
data() {
//这里存放数据
return {
    isshow:false
};
},
//监听属性 类似于data概念
computed: {},
//监控data中的数据变化
watch: {},
//方法集合
methods: {
    addTodos(e){
        // console.log(e.target.value);
        if(e.target.value){
            this.$emit('addEvent',e.target.value);
            e.target.value = '';
        }
    },
    selectAll(){
        this.isshow = !this.isshow;
        this.$emit('allSelect',this.isshow);
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