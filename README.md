# ToDooooo 项目😸

这是一个简单的待办事项应用程序，使用 Vue 3 构建。用户可以添加、编辑、删除和过滤待办事项,供入门新手练手

## 功能👻

- 添加待办事项
- 编辑待办事项
- 删除待办事项
- 过滤待办事项（全部、进行中、已完成）
 
## 核心优化(LightHouse 100/100)🍀
![3ab3ae2ab8a703e73b32a2e551a4972](https://github.com/user-attachments/assets/9efee9bf-4f7c-4c6e-93a4-f3379dcfad17)


## 核心算法

### 添加待办事项🦚

当用户输入待办事项并点击提交按钮或按下回车键时，`addTodo` 函数会被调用。该函数会将新的待办事项添加到 `todolist` 数组的开头，并保存到本地存储。

```javascript
function addTodo() {
  if (item.value.trim() !== "") {
    todolist.unshift({
      id: nanoid(),
      text: item.value,
      completed: false,
      editing: false
    });
    item.value = "";
    saveToLocalStorage();
  } else {
    alert("Please enter the message");
  }
}
```

### 编辑待办事项🌸

当用户点击编辑按钮时，`editTodo` 函数会被调用。该函数会将待办事项的 `editing` 属性设置为 `true`，并保存原始文本。

```javascript
function editTodo(todo) {
  todo.editing = true;
  todo.originalText = todo.text;
}
```

### 保存编辑🪷

当用户完成编辑并失去焦点或按下回车键时，`saveEdit` 函数会被调用。该函数会检查文本是否为空，如果为空，则恢复原始文本。否则，保存编辑并更新本地存储。

```javascript
function saveEdit(todo) {
  if (todo.text.trim() === "") {
    alert("Please enter the message");
    todo.text = todo.originalText;
  }
  todo.editing = false;
  delete todo.originalText;
  saveToLocalStorage();
}
```

### 删除待办事项🌑

当用户点击删除按钮时，`delectTodo` 函数会被调用。该函数会从 `todolist` 数组中移除相应的待办事项，并更新本地存储。

```javascript
function delectTodo(index) {
  todolist.splice(index, 1);
  saveToLocalStorage();
}
```

### 过滤待办事项☀️

用户可以通过点击过滤按钮来过滤待办事项。`setFilter` 函数会被调用以设置当前过滤器。`filteredTodolist` 计算属性会根据当前过滤器返回相应的待办事项。

```javascript
function setFilter(value) {
  filter.value = value;
}

const filteredTodolist = computed(() => {
  if (filter.value === 'all') {
    return todolist;
  } else if (filter.value === 'doing') {
    return todolist.filter(todo => !todo.completed);
  } else if (filter.value === 'completed') {
    return todolist.filter(todo => todo.completed);
  }
  return [];
});
```

### 封装TodoFilter组件,使用$emit通讯🌞
```Vue
<template>
     <div class="sorttodo">
        <button @click="$emit('filter','all')">全部</button>
        <button @click="$emit('filter','doing')">进行中</button>
        <button @click="$emit('filter','done')">已完成</button>
        </div>
</template>

<script setup>

</script>
```

## 项目结构

- App.vue：主组件，包含所有功能和样式。
- `main.js`：入口文件，初始化 Vue 应用程序。

## 安装和运行

1. 克隆项目：
    ```sh
    git clone https://github.com/RADEKWRLD/vue-project.git
    ```
2. 安装依赖：
    ```sh
    npm install
    ```
3. 运行项目：
    ```sh
    npm run serve
    ```

## 贡献

欢迎贡献代码！请提交拉取请求。

## 许可证

本项目基于 MIT 许可证。
```
