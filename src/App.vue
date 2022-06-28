<script setup>
import { defineCustomElement, ref } from "vue";
import { Message } from "equal-vue";
const newTodo = ref("");
const toggleIconsValue = ref("wb_sunny");

const todoData = JSON.parse(localStorage.getItem("todoList")) || [];
const todo = ref(todoData);

function addTodo() {
  if (newTodo.value) {
    todo.value.push({
      isComplete: false,
      todoData: newTodo.value,
    });
    newTodo.value = "";
  }
  saveData();
  Message({
    type: "success",
    text: "Todo added successfully",
  });
}

function isCompleteTodo(todo) {
  todo.isComplete = !todo.isComplete;
  saveData();
  Message({
    type: "warning",
    text: todo.isComplete
      ? "Todo marked as completed"
      : "Todo marked not completed",
  });
}

function removeTodo(index) {
  todo.value.splice(index, 1);
  saveData();
  Message({
    type: "danger",
    text: "Todo is removed",
  });
}

function saveData() {
  const storageData = JSON.stringify(todo.value);
  localStorage.setItem("todoList", storageData);
}
</script>

<template>
  <div class="container mt-4 mb-4">
    <n-space justify="end">
      <it-toggle
        icons
        round
        v-model="toggleIconsValue"
        :options="['wb_sunny', 'bedtime']"
      />
    </n-space>

    <n-space justify="center mb-4">
      <header>TODO APP</header>
    </n-space>
    <n-row gutter="12" class="mt-4 mb-4">
      <n-col :span="10">
        <it-input v-model="newTodo" />
        <!-- label-top="New ToDO"  -->
      </n-col>
      <n-col :span="2">
        <it-button type="primary" @click="addTodo">Add</it-button>
      </n-col>
    </n-row>

    <n-list bordered v-if="todo?.length > 0">
      <n-list-item v-for="(todoItem, index) in todo" :key="index">
        <span
          @click="isCompleteTodo(todoItem)"
          :style="
            todoItem.isComplete ? 'color:gray;text-decoration:line-through' : ''
          "
        >
          {{ todoItem.todoData }}
        </span>

        <template #suffix>
          <n-popconfirm :show-icon="false" @positive-click="removeTodo(index)">
            <template #activator>
              <n-button>Delete</n-button>
            </template>
            Please confirm to delete this todo.
          </n-popconfirm>
        </template>
      </n-list-item>
    </n-list>

    <n-empty size="large" description="No items" v-else>
      <template #extra> </template>
    </n-empty>
  </div>
</template>

