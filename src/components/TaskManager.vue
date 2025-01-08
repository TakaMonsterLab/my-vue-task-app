<template>
  <v-container>
    <v-row>
      <v-col>
        <v-btn @click="openAddTaskDialog" color="primary">追加</v-btn>
      </v-col>
    </v-row>
    <v-list>
      <v-list-item-group>
        <v-list-item>
          <v-list-item-content>
            <v-row>
              <v-col @click="sortTasks('name')">
                <strong>タスク名</strong>
                <v-icon v-if="sortKey === 'name'">{{ sortOrder === 1 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
              </v-col>
              <v-col @click="sortTasks('deadline')">
                <strong>締め切り</strong>
                <v-icon v-if="sortKey === 'deadline'">{{ sortOrder === 1 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
              </v-col>
              <v-col @click="sortTasks('status')">
                <strong>ステータス</strong>
                <v-icon v-if="sortKey === 'status'">{{ sortOrder === 1 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
              </v-col>
              <v-col class="d-flex justify-end"><strong>操作</strong></v-col>
            </v-row>
          </v-list-item-content>
        </v-list-item>
        <v-list-item v-for="(task, index) in sortedTasks" :key="task.id">
          <v-list-item-content>
            <v-row>
              <v-col>{{ task.name }}</v-col>
              <v-col>{{ task.deadline }}</v-col>
              <v-col :class="statusClass(task.status)">{{ task.status }}</v-col>
              <v-col class="d-flex justify-end">
                <v-btn @click="editTask(task)" icon>
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
                <v-btn @click="removeTask(index)" icon>
                  <v-icon>mdi-delete</v-icon>
                </v-btn>
              </v-col>
            </v-row>
          </v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-list>

    <v-dialog v-model="isDialogOpen" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="headline">{{ isEditing ? 'タスク編集' : 'タスク追加' }}</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col>
                <v-text-field v-model="editedTask.name" label="タスク名"></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-text-field v-model="editedTask.deadline" label="締め切り" type="date"></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-select v-model="editedTask.status" :items="statuses" label="ステータス"></v-select>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="isDialogOpen = false">キャンセル</v-btn>
          <v-btn color="blue darken-1" text @click="saveTask">{{ isEditing ? '保存' : '追加' }}</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script setup>
import { ref, computed } from "vue";

const statuses = ["未着手", "対応中", "完了済"];
const tasks = ref([
  { id: 1, name: "タスク1", deadline: "2023-10-01", status: "未着手" },
  { id: 2, name: "タスク2", deadline: "2023-10-02", status: "対応中" },
  { id: 3, name: "タスク3", deadline: "2023-10-03", status: "完了済" },
]);

const sortKey = ref("");
const sortOrder = ref(1);

const isDialogOpen = ref(false);
const isEditing = ref(false);
const editedTask = ref({});

const openAddTaskDialog = () => {
  editedTask.value = { id: null, name: "", deadline: "", status: "" };
  isEditing.value = false;
  isDialogOpen.value = true;
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
};

const sortTasks = (key) => {
  if (sortKey.value === key) {
    sortOrder.value = -sortOrder.value;
  } else {
    sortKey.value = key;
    sortOrder.value = 1;
  }
};

const sortedTasks = computed(() => {
  return [...tasks.value].sort((a, b) => {
    if (a[sortKey.value] < b[sortKey.value]) return -sortOrder.value;
    if (a[sortKey.value] > b[sortKey.value]) return sortOrder.value;
    return 0;
  });
});

const statusClass = (status) => {
  switch (status) {
    case "未着手":
      return "text-danger";
    case "対応中":
      return "text-warning";
    case "完了済":
      return "text-success";
    default:
      return "";
  }
};

const editTask = (task) => {
  editedTask.value = { ...task };
  isEditing.value = true;
  isDialogOpen.value = true;
};

const saveTask = () => {
  if (isEditing.value) {
    const index = tasks.value.findIndex((t) => t.id === editedTask.value.id);
    if (index !== -1) {
      tasks.value[index] = { ...editedTask.value };
    }
  } else {
    const newTask = {
      id: tasks.value.length + 1,
      ...editedTask.value,
    };
    tasks.value.push(newTask);
  }
  isDialogOpen.value = false;
};
</script>

<style scoped>
.text-danger {
  color: red;
}
.text-warning {
  color: orange;
}
.text-success {
  color: green;
}
</style>
