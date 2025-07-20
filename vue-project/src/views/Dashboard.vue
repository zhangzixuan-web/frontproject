<template>
    <div class="dashboard-grid">
      <!-- 第一行: 标题 -->
      <div class="grid-item header">Header</div>
  
      <!-- 第二行: 导航卡片 -->
      <div class="grid-item nav-cards">Nav Cards</div>
      <div class="grid-item todo-list">Todo List</div>
  
      <!-- 第三行 -->
      
      <div class="grid-item progress-tracker">Progress Tracker</div>
      <div class="grid-item countdown">Countdown</div>
  
      <!-- 第四行 -->
      <div class="grid-item calendar">Calendar</div>
      <div class="grid-item habit-heatmap">Habit Heatmap</div>
      <div class="grid-item planner-cards">Planner Cards</div>
  
      <!-- 第五行及以后 -->
      <div class="grid-item daily-updates">Daily Updates</div>
      <div class="grid-item weekly-schedule">Weekly Schedule</div>
      <div class="grid-item time-tracker">Time Tracker</div>
    </div>
  </template>
  
  <script setup lang="ts">
  // 这里是我们的JS/TS逻辑区，现在先留空
  </script>
  
  <style scoped>
/* 定义一些变量，方便统一管理 */
:root {
  --grid-gap: 20px; /* 定义网格项之间的间距 */
  --grid-item-bg: #ffffff; /* 网格项的背景色 */
  --grid-item-border-radius: 12px; /* 网格项的圆角 */
}

.dashboard-grid {
  display: grid; /* 关键！开启网格布局 */
  
  /* 
    定义列：
    我们创建12个列，每个列的宽度是 1fr (fraction，分数单位)
    'repeat(12, 1fr)' 的意思是 "重复12次，每次宽度为1个单位"
    这会自动把所有可用空间平均分成12份，非常灵活和响应式。
  */
  grid-template-columns: repeat(12, 1fr);
  
  /* 
    定义行：
    我们先不固定行高，让内容自动撑开。
    'auto' 意味着行高由其内容决定。
  */
  grid-auto-rows: minmax(100px, auto); /* 每一行最小高度100px，最大自动 */

  /* 定义网格项之间的间距 */
  gap: var(--grid-gap); 
  
  padding: var(--grid-gap); /* 给整个网格容器一个内边距 */
  background-color: #f0f2f5;
  min-height: 100vh;
}

.grid-item {
  background-color: var(--grid-item-bg);
  border-radius: var(--grid-item-border-radius);
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05); /* 加一点阴影，让卡片浮起来 */
  
  /* 以下是为了演示，让文字居中 */
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.2rem;
  color: #888;
}

/* --- 下面开始定义每个模块的位置和大小 --- */

.header {
  /* 
    grid-column: 1 / -1; 
    从第1条网格线开始，到最后一条网格线(-1)结束。
    意思就是：横跨所有12列。
  */
  grid-column: 1 / -1;
}

.nav-cards {
  grid-column: 4 / -1;
}

.todo-list {
  /* 从第1条线开始，跨越3个列，到第4条线结束 */
  grid-column: span 3; 
  grid-row: 2/4;
}

.progress-tracker {
  grid-column: span 6;
}

.countdown {
  grid-column: span 3;
}

.calendar {
  grid-column: span 3;
}

.habit-heatmap {
  grid-column: span 6;
}

.planner-cards {
  grid-column: span 3;
}

.daily-updates {
  grid-column: span 3;
  /* 
    grid-row: span 2;
    这个模块看起来比较高，我们让它纵向跨越2个行单元
  */
  grid-row: span 2; 
}

.weekly-schedule {
  grid-column: span 9;
  grid-row: span 2; 
}

.time-tracker {
  /* 
    这个模块在 daily-updates 的下方，所以我们要明确它的起始位置
    grid-column-start: 1; -> 从第1列开始
    grid-row-start: 6; -> 从第6行开始 (大概估算，可以调整)
  */
  grid-column: span 3;
  /* 注意：这里的行号可能需要根据实际内容高度微调 */
}
</style>