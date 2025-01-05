<template>
  <v-container>
    <v-row>
      <v-col>
        <v-text-field
          v-model="newTask"
          label="新しいタスクを追加"
          @keyup.enter="addTask"
        />
        <v-btn @click="addTask">追加</v-btn>
      </v-col>
    </v-row>
    <v-list>
      <v-list-item-group>
        <v-list-item v-for="(task, index) in tasks" :key="index">
          <v-list-item-content>
            <v-list-item-title>{{ task }}</v-list-item-title>
          </v-list-item-content>
          <v-list-item-action>
            <v-btn @click="removeTask(index)" icon>
              <v-icon>mdi-delete</v-icon>
            </v-btn>
          </v-list-item-action>
        </v-list-item>
      </v-list-item-group>
    </v-list>
  </v-container>
</template>

<script setup>
import { ref } from "vue";

const newTask = ref("");
const tasks = ref([]);

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push(newTask.value.trim());
    newTask.value = "";
  }
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
};
</script>

<style scoped>
/* スタイルをここに追加できます */
</style>
