<!-- Html -->
<template>
  <div class="container flex justify-center items-center">
    <div class="flex flex-col gap-1">
      <h1 class="text-center text-2xl text-slate-200">Create Todo List</h1>
      <div
        class="flex flex-row justify-between mt-3 sm:mt-5 items-center gap-4"
      >
        <input
          type="text"
          class="py-2 px-4 rounded-md"
          placeholder="Add a new task..."
          v-model="newTask"
          @keyup.enter="addTask"
          :disabled="tasks.length >= 10"
        />
        <button
          type="button"
          class="bg-green-300 py-2 px-4 rounded-md"
          @click="addTask"
          v-if="tasks.length <= 9"
        >
          Add
        </button>
        <p v-else class="text-orange-700">Жагсаалт дүүрсэн байна</p>
      </div>
      <div class="mt-3 sm:mt-5">
        <div class="mt-2" v-for="(task, index) in tasks" :key="index">
          <div class="flex justify-between items-center">
            <input class="bg-slate-300" type="checkbox" v-model="task.isDone" />
            <div>
              <p class="font-semibold text-slate-200">
                {{ task.description }}
              </p>
            </div>
            <div>
              <button
                type="button"
                class="bg-slate-300 py-2 px-4 rounded-md"
                @click="deleteTask(index)"
              >
                Устгах
              </button>
            </div>
          </div>
        </div>

        <hr />
        <div class="flex flex-col justify-center items-center mt-5 gap-3">
          <div>
            <p class="text-lg text-orange-300" v-show="pendingTasks > 0">
              * Таны нийт даалгаварын тоо - {{ pendingTasks }}
            </p>
          </div>
          <div>
            <button
              type="button"
              class="bg-red-400 py-2 px-4 rounded-md"
              @click="deleteAllTasks"
              v-show="tasks.length > 0"
            >
              Бүх даалгаварийг устгах
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, onMounted, computed, watch } from "vue";

export default {
  name: "HomePage",

  setup() {
    const newTask = ref("");
    const tasks = ref([
      { description: "1:30 багийн уулзалттай", isDone: false },
    ]);

    const addTask = () => {
      if (!newTask.value) return;
      tasks.value.unshift({
        description: newTask.value,
        isDone: false,
      });
      localStorage.setItem("tasks", JSON.stringify(tasks.value));
      newTask.value = "";
    };

    const deleteTask = (index) => {
      tasks.value.splice(index, 1);
      localStorage.setItem("tasks", JSON.stringify(tasks.value));
    };

    const deleteAllTasks = () => {
      tasks.value = [];
      localStorage.removeItem("tasks");
    };

    const pendingTasks = computed(() => {
      return tasks.value.filter((e) => e.isDone === false).length;
    });

    watch(
      tasks,
      () => {
        if (tasks.value.length >= 10) {
          alert(
            "Жагсаалтын хязгаар 10т хүрлээ. Та өмнөх даалгавараа дуугаж устгана уу?"
          );
        }
      },
      { deep: true }
    );

    onMounted(() => {
      if (localStorage.tasks) {
        tasks.value = JSON.parse(localStorage.getItem("tasks")) || [];
      }
    });

    return {
      newTask,
      tasks,
      addTask,
      deleteTask,
      deleteAllTasks,
      pendingTasks,
    };
  },
};
</script>
