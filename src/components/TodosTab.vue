<!-- todos的底部tab栏 -->
<template>
<div class='tab'>
    <div class="tab-left">
        {{remaining}} item left
    </div>
    <div class="tab-center">
        <span v-for="(item,index) in tabArr" :key="item" :class="{active:index===currentIndex}" @click="tabChange(index)">{{ item }}</span>
    </div>
    <div :class="['tab-right',{hidden:!todos.filter(e => e.completed).length}]" @click="removeCompleted">
        Clear completed
    </div>
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
    tabArr:['All','Active','Completed'],
    currentIndex:0
};
},
props:{
    todos:{
        type:Array,
        default:function(){
            return []
        }
    }
},
//监听属性 类似于data概念
computed: {
    // 剩余多少项未完成
    remaining(){
        return this.todos.filter( element => !element.completed ).length;
    }
},
//监控data中的数据变化
watch: {},
//方法集合
methods: {
    // 切换tab选项，查看所有、未完成、已完成事项
   tabChange(index){
       this.currentIndex = index;
       this.$emit('tabChange',this.tabArr[index])
   },
    // 删除所有已完成的事项
   removeCompleted(){
       this.$emit('removeCompleted')
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
    .tab{
        width: 550px;
        height: 50px;
        display: flex;
        justify-content: space-between;
        padding: 0 15px;
        align-items: center;
        color: #777;
        font-size: 14px;
    }
    .tab-center span{
        padding: 0 7px;
        margin: 0 3px;
        cursor: pointer;
        border: 1px solid transparent;
    }
    .tab-right{
        cursor: pointer;
        text-decoration: underline;
    }
    .tab-center span:hover{
        border-color: rgba(175, 47, 47, 0.1);
    }
    .active{
        /* border: 1px solid rgba(175, 47, 47, 0.2); */
        border-color: rgba(175, 47, 47, 0.2)!important;
    }
    .hidden{
        visibility: hidden;
    }
</style>