<template>
  <div class="hello">
  <h1>{{ message }}</h1>
  <ul>
    <!-- 遍历 items 数组，显示每个元素的内容 -->
    <li v-for="item in items" :key="item.id">{{ item.name }}</li>
  </ul>
</div>
</template>

<script>
// 导入 DataService
   // 导入 ref 和 onMounted
   import { ref, onMounted } from 'vue';
   import DataService from '../services/DataService.js';

   export default {
     name: 'HelloWorld',
     setup() {
       // 初始化数据
       const message = ref('');
       const items = ref([]);
       //定义一个异步函数来获取数据
       const fetchData = async () => {
         try {
           const response = await DataService.getData();
           // 将获取的数据设置为组件的 data 属性
           message.value = response.data.message;
           items.value = response.data.items;
         }catch(error){
           console.error(error);
         }
       };
       // 在组件挂载时调用 fetchData 函数
       onMounted(fetchData);
       // 返回数据，以便在模板中使用
       return {message, items};
     },
   };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
