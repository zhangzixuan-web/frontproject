<template>
    <div class="todo-list-widget">
      <h3 class="widget-title">快捷操作 (Todo)</h3>
      
      <div class="add-todo">
       
        <input 
          v-model="newTodoText" 
          type="text" 
          placeholder="添加一个新的待办事项..."
          @keyup.enter="addTodo"
        />
        
        <button @click="addTodo">+</button>
      </div>
      
      <ul class="todo-items">
        <!-- 点击整行来切换完成状态 -->
        <li 
          v-for="todo in todos" 
          :key="todo.id" 
          :class="{ completed: todo.completed }"
          @click="toggleComplete(todo.id)"
        >
          <span class="todo-text">{{ todo.text }}</span>
          
          <button class="delete-btn" @click.stop="deleteTodo(todo.id)">×</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue';
  
  // 定义待办事项的数据类型
  interface Todo {
    id: number;
    text: string;
    completed: boolean;
  }
  
  // 1. 新增一个 ref 来绑定输入框的内容
  const newTodoText = ref('');
  
  // 2. 创建一个响应式的待办事项列表（使用假数据）
  const todos = ref<Todo[]>([
    { id: 1, text: '学习 Vue 3 Composition API', completed: true },
    { id: 2, text: '完成仪表盘网格布局', completed: true },
    { id: 3, text: '开始制作 TodoList 组件', completed: false },
    { id: 4, text: '喝一杯水', completed: false },
  ]);
  
  // 3. 添加新待办事项的函数
  function addTodo() {
    // 如果输入为空，则不执行任何操作
    if (!newTodoText.value.trim()) return;
  
    // 创建一个新的 todo 对象
    const newTodo: Todo = {
      id: Date.now(), // 使用时间戳作为唯一的ID
      text: newTodoText.value,
      completed: false,
    };
    
    // 将新的 todo 添加到列表的开头
    todos.value.unshift(newTodo);
    
    // 清空输入框
    newTodoText.value = '';
  }
  
  // 4. 切换待办事项完成状态的函数
  function toggleComplete(id: number) {
    const todo = todos.value.find(t => t.id === id);
    if (todo) {
      todo.completed = !todo.completed;
    }
  }
  
  // 5. 删除待办事项的函数
  function deleteTodo(id: number) {
    // 找到要删除的 todo 的索引
    const index = todos.value.findIndex(t => t.id === id);
    if (index !== -1) {
      // 从数组中移除它
      todos.value.splice(index, 1);
    }
  }
  </script>
  
  <style scoped>
.todo-list-widget {
  display: flex;
  flex-direction: column;
  height: 100%;
  background-color: #fff; /* 确保背景色是白色 */
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.widget-title {
  margin-top: 0;
  margin-bottom: 20px;
  font-size: 1.2rem;
  font-weight: 600; /* 标题加粗 */
  color: #2c3e50; /* 使用更柔和的黑色 */
}

/* --- 输入框美化 --- */
.add-todo {
  display: flex;
  margin-bottom: 15px;
}

.add-todo input {
  flex-grow: 1;
  border: 1px solid #e0e0e0;
  padding: 10px 14px;
  border-radius: 8px; /* 统一圆角 */
  font-size: 0.9rem;
  transition: all 0.2s ease-in-out; /* 添加平滑过渡效果 */
  margin-right: 10px; /* 和按钮拉开距离 */
}
.add-todo input:focus {
  outline: none;
  border-color: #4a90e2; /* 蓝色高亮 */
  box-shadow: 0 0 0 2px rgba(74, 144, 226, 0.2); /* 辉光效果 */
}

.add-todo button {
  border: none;
  background-color: #4a90e2;
  color: white;
  padding: 0 16px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1.5rem; /* 增大“+”号 */
  font-weight: 300;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.2s;
}
.add-todo button:hover {
  background-color: #357abd; /* 悬停时颜色变深 */
}


/* --- 列表项美化 --- */
.todo-items {
  list-style: none;
  padding: 0;
  margin: 0;
  flex-grow: 1;
  overflow-y: auto;
}

/* 给滚动条也美化一下（仅在Webkit内核浏览器生效，如Chrome/Edge） */
.todo-items::-webkit-scrollbar {
  width: 6px;
}
.todo-items::-webkit-scrollbar-thumb {
  background: #dcdcdc;
  border-radius: 3px;
}
.todo-items::-webkit-scrollbar-thumb:hover {
  background: #b3b3b3;
}

.todo-items li {
  display: flex;
  align-items: center;
  padding: 12px 5px;
  border-bottom: 1px solid #f2f2f2;
  transition: background-color 0.2s;
}
.todo-items li:hover {
  background-color: #f9f9f9;
}
.todo-items li:last-child {
  border-bottom: none; /* 最后一项不要下边框 */
}

/* 自定义 Checkbox 样式 */
.todo-items li::before {
  content: '✓';
  display: inline-block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  border: 1px solid #ccc;
  text-align: center;
  line-height: 20px;
  margin-right: 15px;
  cursor: pointer;
  color: transparent; /* 默认不显示✓ */
  transition: all 0.2s;
}
.todo-items li.completed::before {
  background-color: #67c23a;
  border-color: #67c23a;
  color: white; /* 完成时显示✓ */
}

.todo-text {
  flex-grow: 1;
  transition: color 0.2s;
}

.todo-items li.completed .todo-text {
  text-decoration: line-through;
  color: #b8b8b8;
}

.delete-btn {
  border: none;
  background: none;
  cursor: pointer;
  color: #ccc;
  font-size: 1.2rem;
  opacity: 0; /* 默认隐藏删除按钮 */
  transition: all 0.2s;
}
.todo-items li:hover .delete-btn {
  opacity: 1; /* 鼠标悬停在列表项上时才显示 */
  color: #f56c6c;
}
</style>