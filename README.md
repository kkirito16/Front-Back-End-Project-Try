# 基于vue3＆flask的前后端分离demo

选择flask的目的：灵活，代码库小，轻量，基于python ＆ 算法侧使用的是python语言环境

前端：

HelloWorld.vue

```vue
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
```

DataService.js

定义一个可以用来获取数据的函数，并配置了axios实例的基础URL和请求头信息

```js
// 导入 axios
import axios from 'axios';
// 创建一个 axios 实例，用于发送请求
const apiClient = axios.create({
  // 设置后端 API 的基础 URL
  baseURL: 'http://localhost:5000/api',
  // 设置请求头
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json',
  },
});

// 定义一个用于获取数据的函数
export default {
  getData() {
    // 向 '/data' 路由发送 GET 请求
    return apiClient.get('/data');
  },
};
```

后端：

```python
# 导入 Flask
from flask import Flask, jsonify
# 导入 CORS
from flask_cors import CORS

# 创建一个 Flask 应用实例
app = Flask(__name__)
# 并允许来自所有域的请求
CORS(app)


# 定义一个简单的路由
@app.route('/api/data', methods=['GET'])
def get_data():
    # 创建一个字典作为模拟数据
    data = {
        "message": "Hello from Flask!!",
        "items": [
            {"id": 1, "name": "Item 1"},
            {"id": 2, "name": "Item 2"},
            {"id": 3, "name": "Item 3"}
        ]
    }
    # 将字典转为 JSON 并返回
    return jsonify(data)


# 如果作为主程序运行，启动应用
if __name__ == '__main__':
    app.run(debug=True)
```

前端代码使用axios或发送GET请求到后端提供的API端点，如`http://localhost:5000/api/data`。后端应用会接收到请求并返回**JSON数据**作为响应。前端收到响应后，可以解析JSON数据并在页面上展示或处理这些数据。

![image-20240318101604163](https://cdn.jsdelivr.net/gh/kkirito16/ImgPicGo/img/image-20240318101604163.png)

![image-20240318101641491](https://cdn.jsdelivr.net/gh/kkirito16/ImgPicGo/img/image-20240318101641491.png)