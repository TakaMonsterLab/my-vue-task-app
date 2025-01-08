<template>
  <v-container>
    <v-row>
      <v-col>
        <v-text-field v-model="newTaskName" label="タスク名"></v-text-field>
      </v-col>
      <v-col>
        <v-text-field v-model="newTaskDeadline" label="締め切り" type="date"></v-text-field>
      </v-col>
      <v-col>
        <v-select v-model="newTaskStatus" :items="statuses" label="ステータス"></v-select>
      </v-col>
      <v-col>
        <v-btn @click="addTask" color="primary">追加</v-btn>
      </v-col>
    </v-row>
    <v-list>
      <v-list-item-group>
        <v-list-item v-for="(task, index) in tasks" :key="task.id">
          <v-list-item-content>
            <v-row>
              <v-col>{{ task.name }}</v-col>
              <v-col>{{ task.deadline }}</v-col>
              <v-col>{{ task.status }}</v-col>
              <v-col class="d-flex justify-end">
                <v-btn @click="removeTask(index)" icon>
                  <v-icon>mdi-delete</v-icon>
                </v-btn>
              </v-col>
            </v-row>
          </v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-list>
  </v-container>
</template>

<script setup>
import { ref } from "vue";

const newTaskName = ref("");
const newTaskDeadline = ref("");
const newTaskStatus = ref("");
const statuses = ["未着手", "対応中", "完了済"];
const tasks = ref([
  { id: 1, name: "タスク1", deadline: "2023-10-01", status: "未着手" },
  { id: 2, name: "タスク2", deadline: "2023-10-02", status: "対応中" },
  { id: 3, name: "タスク3", deadline: "2023-10-03", status: "完了済" },
]);

const addTask = () => {
  if (newTaskName.value.trim() && newTaskDeadline.value && newTaskStatus.value) {
    const newTask = {
      id: tasks.value.length + 1,
      name: newTaskName.value.trim(),
      deadline: newTaskDeadline.value,
      status: newTaskStatus.value,
    };
    tasks.value.push(newTask);
    newTaskName.value = "";
    newTaskDeadline.value = "";
    newTaskStatus.value = "";
  }
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
};
</script>

<style scoped>
/* スタイルをここに追加できます */
</style>
