<!--
 * @Description: 
 * @Version: 1.0
 * @Autor: Li Cheng
 * @Date: 2024-06-24 14:34:13
 * @LastEditors: Li Cheng
 * @LastEditTime: 2024-06-24 15:01:55
-->
<template>
  <div class="container">
    <h1>用户列表</h1>

    <!-- 添加用户按钮，点击展开表单 -->
    <button class="toggle-button" @click="toggleFormVisible">
      {{ formVisible ? '收起添加用户表单' : '添加用户' }}
    </button>

    <!-- 添加用户表单 -->
    <transition name="fade">
      <form v-if="formVisible" class="form-container" @submit.prevent="addUser">
        <h2>添加用户</h2>
        <label for="userId">学号：</label>
        <input type="text" id="userId" v-model="newUser.id" required>
        <label for="userName">姓名：</label>
        <input type="text" id="userName" v-model="newUser.userName" required>
        <label for="userAge">年龄：</label>
        <input type="number" id="userAge" v-model.number="newUser.age" required>
        <button type="submit">添加</button>
      </form>
    </transition>

    <hr>

    <!-- 用户信息列表 -->
    <div class="table-container">
      <table>
        <thead>
          <tr>
            <th>#</th>
            <th>学号</th>
            <th>姓名</th>
            <th>年龄</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in list" :key="item.id">
            <td>{{ index + 1 }}</td>
            <td>{{ item.id }}</td>
            <td>{{ item.userName }}</td>
            <td>{{ item.age }}</td>
            <td class="button-group">
              <button @click="deleteUser(index)">删除</button>
              <button @click="startEditUser(index)">编辑</button>
              <button @click="getYourName(item.id)">问名字</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- 编辑用户表单 -->
    <transition name="fade">
      <form v-if="editFormVisible" class="form-container" @submit.prevent="updateUser">
        <h2>编辑用户</h2>
        <label for="editUserId">学号：</label>
        <input type="text" id="editUserId" v-model="editUser.id" disabled>
        <label for="editUserName">姓名：</label>
        <input type="text" id="editUserName" v-model="editUser.userName" required>
        <label for="editUserAge">年龄：</label>
        <input type="number" id="editUserAge" v-model.number="editUser.age" required>
        <button type="submit">更新</button>
      </form>
    </transition>
  </div>
</template>

<script setup>
import { reactive, ref, onMounted, onBeforeUnmount } from 'vue';

// 从localStorage加载用户列表数据
const loadListFromLocalStorage = () => {
  const savedList = localStorage.getItem('userList');
  return savedList ? JSON.parse(savedList) : [];
};

// 将用户列表数据保存到localStorage
const saveListToLocalStorage = (list) => {
  localStorage.setItem('userList', JSON.stringify(list));
};

// 使用reactive创建响应式的列表数据
const list = reactive(loadListFromLocalStorage());

// 初始化列表数据
onMounted(() => {
  if (list.length === 0) {
    list.push(
      { id: 220812022, userName: '刘淑文', age: 18 },
      { id: 220812008, userName: '蔡欣怡', age: 18 },
      { id: 220812030, userName: '龙依', age: 18 }
    );
    saveListToLocalStorage(list); // 初始化后保存到localStorage
  }
});

// 删除用户
const deleteUser = index => {
  list.splice(index, 1);
  saveListToLocalStorage(list); // 删除后保存到localStorage
};

// 启动编辑用户
const startEditUser = index => {
  editUser.index = index;
  editUser.id = list[index].id;
  editUser.userName = list[index].userName;
  editUser.age = list[index].age;
  editFormVisible.value = true;
};

// 更新用户
const updateUser = () => {
  const index = editUser.index;
  if (index !== null) {
    const updatedUser = {
      id: editUser.id,
      userName: editUser.userName,
      age: editUser.age,
    };
    list.splice(index, 1); // 删除旧位置的用户
    list.unshift(updatedUser); // 将更新后的用户添加到列表的第一条
    saveListToLocalStorage(list); // 编辑后保存到localStorage
    editFormVisible.value = false;
  }
};

// 询问用户姓名
const getYourName = id => {
  const student = list.find(item => item.id === id);
  alert(student.userName); // 弹出用户的姓名信息
};

// 添加用户
const newUser = reactive({
  id: '',
  userName: '',
  age: ''
});

const addUser = () => {
  if (newUser.id && newUser.userName && newUser.age) {
    list.unshift({
      id: newUser.id,
      userName: newUser.userName,
      age: parseInt(newUser.age)
    });
    saveListToLocalStorage(list); // 添加后保存到localStorage
    newUser.id = '';
    newUser.userName = '';
    newUser.age = '';
    toggleFormVisible(); // 添加后收起表单
  } else {
    alert('请填写完整信息');
  }
};

// 控制添加表单的显示和隐藏
const formVisible = ref(false);
const toggleFormVisible = () => {
  formVisible.value = !formVisible.value;
};

// 控制编辑表单的显示和隐藏
const editFormVisible = ref(false);
const editUser = reactive({
  index: null,
  id: '',
  userName: '',
  age: ''
});

// 在页面卸载前保存列表状态
onBeforeUnmount(() => {
  saveListToLocalStorage(list);
});
</script>

<style lang="less"scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.toggle-button {
  display: block;
  margin: 20px auto;
}

.form-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 20px auto;
  border: 1px solid #ccc;
  padding: 10px;
  max-width: 400px;
}

label {
  display: inline-block;
  width: 80px;
  margin-bottom: 5px;
}

input[type="text"],
input[type="number"] {
  width: calc(100% - 90px);
  padding: 5px;
  margin-bottom: 10px;
}

button {
  padding: 5px 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  cursor: pointer;
  margin-right: 5px;
}

button[type="submit"] {
  background-color: #28a745;
}

.button-group {
  display: flex;
  gap: 5px;
}

.table-container {
  max-width: 600px;
  margin: 20px auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: left;
}

th {
  background-color: #f8f8f8;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

hr {
  border: none;
  border-top: 1px solid #ccc;
  margin: 20px 0;
}
</style>


