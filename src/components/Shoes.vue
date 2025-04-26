<template>
  <div class="shoe-table-container">
    <h1>鞋子编号</h1>
    
    <!-- 添加表单 -->
    <div class="add-form">
      <div class="form-row">
        <div class="form-group">
          <label>品牌编号:</label>
          <input  v-model="newShoe.id" placeholder="例如: 1001" />
        </div>
        <div class="form-group">
          <label>品牌名称:</label>
          <input  v-model="newShoe.name" placeholder="例如: 亚瑟士" />
        </div>
        <div class="form-group">
          <label>单价:</label>
          <input  v-model="newShoe.price" placeholder="例如: 223" />
        </div>
        <div class="form-group">
          <label>库存数量:</label>
          <input v-model="newShoe.number" placeholder="例如: 50" />
        </div>
        <button  @click.prevent="addShoe" class="add-btn">添加</button>
      </div>
    </div>

    <!-- 查询框 -->
     <input v-model="searchName" type="text" placeholder="请输入品牌名称" >

    <!-- 鞋子数据表格 -->
    <table class="shoe-table">
      <thead>
        <tr>
          <th>鞋子编号</th>
          <th>品牌名称</th>
          <th>单价</th>
          <th>库存数量</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="shoe in filteredShoes" :key="shoe.id"> <!-- shoe是变量名，shoes是数组名-->
          <td>{{ shoe.id }}</td>
          <td>{{ shoe.name }}</td>
          <td>{{ shoe.price }}</td>
          <td>{{ shoe.number }}</td>
          <td>
            <button @click="defineShoes(shoe.id)" class="delete-btn">删除</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup name="Shoes">
import { reactive, ref, computed } from 'vue'

  // 定义鞋子数据
  let shoes = reactive( [
    { id: 1001, name: '亚瑟士', price: 223, number: 50 },
    { id: 1002, name: '耐克', price: 500, number: 30 },
    { id: 1003, name: '阿迪达斯', price: 450, number: 20 }
  ])

  function defineShoes(id) {
    let index = shoes.findIndex(shoe => shoe.id === id); //findIndex()方法返回数组中满足提供的测试函数的第一个元素的索引
    if (index !== -1) {
      shoes.splice(index, 1); // splice() 方法从数组中添加或删除项目
    }
    // 删除鞋子数据的逻辑
    console.log('删除鞋子数据')
  }

  //添加数据
  let newShoe = {
    id: '',
    name: '',
    price: '',
    number: ''
  };

  function addShoe(){
    shoes.push(newShoe);
  }

  //查询功能
  let searchName = ref('')

  // 计算属性，用于过滤鞋子数据
  const filteredShoes = computed(() => {
    if (!searchName) {
      return shoes; // 如果没有输入，返回所有鞋子数据
    }
    return shoes.filter(
        shoe => shoe.name.toLowerCase().includes(searchName.value.toLowerCase())
    )
  });

</script>

<style scoped>
  .shoe-table-container {
    font-family: Arial, sans-serif;
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
  }

  h1 {
    text-align: center;
    margin-bottom: 20px;
  }

  .add-form {
    margin-bottom: 20px;
    padding: 15px;
    background-color: #f5f5f5;
    border-radius: 5px;
  }

  .form-row {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    align-items: flex-end;
  }

  .form-group {
    display: flex;
    flex-direction: column;
  }

  .form-group label {
    margin-bottom: 5px;
    font-size: 14px;
  }

  .form-group input {
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    min-width: 100px;
  }

  .add-btn {
    padding: 8px 16px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    height: 36px;
  }

  .add-btn:hover {
    background-color: #45a049;
  }

  .shoe-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
  }

  .shoe-table th, .shoe-table td {
    border: 1px solid #ddd;
    padding: 12px;
    text-align: center;
  }

  .shoe-table th {
    background-color: #f2f2f2;
    font-weight: bold;
  }

  .shoe-table tr:nth-child(even) {
    background-color: #f9f9f9;
  }

  .shoe-table tr:hover {
    background-color: #f1f1f1;
  }

  .delete-btn {
    padding: 6px 12px;
    background-color: #f44336;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .delete-btn:hover {
    background-color: #da190b;
  }
</style>