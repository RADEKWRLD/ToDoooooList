<template>
   <a href="https://github.com/RADEKWRLD" target="_blank" class="github-link">
          <svg width="48" height="48" viewBox="0 0 24 24" fill="black">
            <path d="M12 0C5.373 0 0 5.373 0 12c0 5.302 3.438 9.8 8.207 11.387.6.113.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23A11.509 11.509 0 0 1 12 5.803c1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576C20.566 21.797 24 17.3 24 12c0-6.627-5.373-12-12-12z"/>
          </svg>
        </a>
  <h1>ToDooooo</h1>
  <div class="textbox">
    <input placeholder="What you want to dooooo?" type="text" name="text" id="text" v-model="item" @keydown.enter="addTodo">
    <button @click="addTodo">提交</button>
  </div>
  <div class="sorttodo">
    <button @click="setFilter('all')">全部</button>
    <button @click="setFilter('doing')">进行中</button>
    <button @click="setFilter('completed')">已完成</button>
  </div>
  <div class="listcontent">
    <ul>
      <li v-for="(todo, index) in sortedTodolist" :key="todo.id">
        <input type="checkbox" v-model="todo.completed" @change="saveToLocalStorage">
        <span v-if="!todo.editing" :class="{ completed: todo.completed }">{{ todo.text }}</span>
        <input v-else type="text" v-model="todo.text" @blur="saveEdit(todo)" @keyup.enter="saveEdit(todo)" @keyup.esc="cancelEdit(todo)">
        <div class="button-group">
          <button class="editbtn" @click="editTodo(todo)">编辑</button>
          <button class="deletebtn" @click="delectTodo(index)">删除</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { nanoid } from 'nanoid';
import { reactive, ref, computed, onMounted } from 'vue';

const todolist = reactive([]);
const item = ref("");
const filter = ref('all');

onMounted(() => {
  const savedTodos = localStorage.getItem('todolist');
  if (savedTodos) {
    todolist.push(...JSON.parse(savedTodos));
  }
});

function saveToLocalStorage() {
  localStorage.setItem('todolist', JSON.stringify(todolist));
}

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

function editTodo(todo) {
  todo.editing = true;
  todo.originalText = todo.text;
}

function saveEdit(todo) {
  if (todo.text.trim() === "") {
    alert("Please enter the message");
    todo.text = todo.originalText;
  }
  todo.editing = false;
  delete todo.originalText;
  saveToLocalStorage();
}

function cancelEdit(todo) {
  todo.text = todo.originalText;
  todo.editing = false;
  delete todo.originalText;
}

function delectTodo(index) {
  todolist.splice(index, 1);
  saveToLocalStorage();
}

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

const sortedTodolist = computed(() => {
  return [...filteredTodolist.value].sort((a, b) => a.completed - b.completed);
});
</script>

<style scoped>


html {
  font-family: 'Segoe UI', system-ui, sans-serif;
  background-color: #2D336B;
}

h1 {
  margin-top: -2rem;
  text-align: center;
  font-size: 5rem;
  letter-spacing: 3px;
  background: linear-gradient(45deg, #00f7ff, #0066ff);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  margin-bottom: 1.3rem;
}

/* 输入框区域 */
.textbox {
  display: flex;
  gap: 1rem;
  max-width: 600px;
  margin: 0 auto 2rem;
}

input[type="text"] {
  flex: 1;
  padding: 1rem;
  border: 2px solid #3a3939;
  border-radius: 8px;
  color: #0c0c0c;
  font-size: 1rem;
  transition: all 0.2s ease;
}

input[type="text"]:focus {
  outline: none;
  border-color: #0066ff;
  box-shadow: 0 0 15px rgba(0, 103, 255, 0.2);
}

input[type="text"]::placeholder {
  color: #a9b4bf;
} 

button {
  padding: 0.8rem 1.5rem;
  background: linear-gradient(135deg, #0066ff 0%, #00f7ff 100%);
  border: none;
  border-radius: 8px;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 800;
}

button:hover {
  transform: scale(2px);
  box-shadow: 0 0px 8px #363c77;
}

.listcontent {
  padding: 1rem;
  border: 2px solid #0c0c0c;
  border-radius: 8px;
  margin: 2rem auto;
  max-width: 600px;
  min-height: 50vh;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1.2rem;
  margin: 1rem 0;
  background: #fff;
  border-radius: 12px;
  border: 1px solid #a9b4bf;
  transition: all 0.3s ease;
}

li:hover {
  background: #d8d6d6;
  transform: translateY(-3px);
}

/* 按钮组样式 */
.button-group {
  display: flex;
  gap: 0.5rem;
  margin-left: auto;
}

.deletebtn, .editbtn {
  padding: 0.5rem 1rem;
  background: #ff4d4d;
  border: none;
  border-radius: 8px;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
}

.editbtn {
  background: #4d79ff;
}

.deletebtn:hover, .editbtn:hover {
  transform: scale(2px);
  box-shadow: 0 0px 5px #2D336B;
}

/* 复选框样式 */
input[type="checkbox"] {
  width: 20px;
  height: 20px;
  border: 2px solid #0066ff;
  border-radius: 5px;
  appearance: none;
  cursor: pointer;
  transition: all 0.2s ease;
  word-wrap: break-word;
}

input[type="checkbox"]:checked {
  background: #0066ff;
  border-color: #0066ff;
  position: relative;
}

input[type="checkbox"]:checked::after {
  content: "✓";
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  color: white;
}

.completed {
  color: #5B913B;
  text-decoration: line-through;
}

/* 过滤按钮样式 */  
.sorttodo {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin: 2rem 0;
  max-width: 600px;
  margin: 0 auto;
}

.sorttodo button {
  margin: 0rem 50px;
  background: #3a3939;
  border: 1px solid #0c0c0c;
  padding: 0.6rem 1.5rem;
}



/* 编辑输入框 */
input[type="text"][v-if] {
  background: transparent;
  border: none;
  padding: 0;
  font-size: inherit;
}

input[type="text"][v-if]:focus {
  box-shadow: none;
}

/*响应式设计*/
   
@media (max-width: 600px) {
  h1 {
    font-size: 3rem;
  }

  svg{
    width: 30px;
    height: 30px;
  }

  .textbox {
    flex-direction: row;
    gap: 0.5rem;
  }

  .textbox input[type="text"] {
    height: 100%;
    font-size: 1rem;
  }

  button {
 
    font-size: 1rem;
  }

  .listcontent {
    padding: 0.5rem;
    margin: 1rem auto;
  }

  li {
    flex-direction: row;
    align-items: center;
    gap: 0.5rem;
    padding: 0.8rem;
  }

  .button-group {
    width: 100%;
    justify-content: flex-end;
  }

  .sorttodo {
    flex-direction: column;
    gap: 0.5rem;
  }

  .sorttodo button {
    margin: 0;
    padding: 0.5rem 1rem;
  }
}
</style>
