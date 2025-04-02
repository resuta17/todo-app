<script setup>
import { ref } from "vue";

const toDoList = ref([]);
const whatToDo = ref("");
const inputRef = ref(null);


const addToDO = () => {
  if (whatToDo.value.trim() === "") {
    alert("Please enter a todo!");
  } else {

    const now = new Date();
    const date = now.toLocaleString();
    const nextId = toDoList.value.length > 0 ? Math.max(...toDoList.value.map(todo => todo.id)) + 1 : 1;

    toDoList.value.push({
        id: nextId,
      task: whatToDo.value,
      date: date,
      hidden: false
    });

    whatToDo.value = "";
  }
};

const focusInput = () => {
  inputRef.value?.focus();
};

const removeTodo = (index) => {
    toDoList.value.splice(index, 1);
};
</script>

<template>
    <div class="flex items-center justify-center min-h-screen p-6 bg-black">
      <div class="w-full max-w-xl">

        <div class="flex flex-col items-center">
          <img
            src="./assets/logo-9fb5691f.jpg"
            alt="Logo"
            class="object-cover w-full max-w-xl mb-6 border border-white rounded"
          >

          <h3 class="w-full max-w-xl mb-6 text-4xl font-bold text-left text-white">
            TODO
          </h3>

          <div class="flex items-center w-full max-w-xl space-x-4">
            <input
              placeholder="Enter your todo here"
              type="text"
              class="w-full h-12 px-4 text-lg border border-gray-300 rounded focus:outline-none"
              v-model="whatToDo"
            >

            <button
              @click="addToDO"
              class="w-1/3 h-12 px-6 text-lg font-bold text-black transition bg-white rounded-sm"
            >
              Add Todo
            </button>
          </div>
        </div>
        <div v-if="toDoList.length"
            class="w-full max-w-xl p-2 mx-auto mt-2 bg-gray-700 border rounded-md"
            :class="{'h-80 overflow-y-auto': toDoList.length > 3, 'h-auto': toDoList.length <= 2}"
        >
            <div
              v-for="(item, index) in toDoList"
              :key="index"
              class="flex items-center justify-between p-2 text-white bg-gray-700 rounded"
            >
              <div class="w-full">
                <div class="flex items-center w-full font-bold">
                  <label class="flex-1 text-left">Todo# {{ item.id }}</label>
                  <button
                    @click="removeTodo(index)"
                    class="px-2 py-1 ml-auto text-white rounded"
                  >
                    x
                  </button>
                </div>
                    <p 
                    v-if="!item.hidden" 
                        @click="item.hidden = true" 
                        class="mb-2 text-left cursor-pointer"
                        >
                        {{ item.task }}
                    </p>
                    <div v-else
                        @click="focusInput"
                        @keydown.enter="item.hidden = false">
                        <input ref="edit"
                        v-model="item.task"
                        autofocus 
                        class="w-full px-2 py-1 text-left text-gray-900 border rounded"
                        >
                        <p class="text-[10px] italic mb-2">Press enter to update</p>     
                    </div>
                    
                <div class="flex justify-between w-full">
                    <p class="w-full text-xs font-bold tracking-widest text-right text-gray-400 ">
                        {{ item.date }}
                    </p>
                </div>
                <hr class="mt-2 border-t-2 border-gray-300">
            </div>
            </div>
        </div>
        <div v-else class="mt-2 border border-white rounded-md text-white p-2 max-h-[400px] overflow-auto">
            <p class="text-center">Nothing to Display.</p>
          </div>
      </div>
    </div>
  </template>


