<script>
export default {
    props: {
        task: Object,
        type: Object
  },
  methods: {
    taskDrag(id) {
        this.$emit('dragTask', id);
    },
    editTask(id) {
        this.$emit('editTask', id);
    }
  }
}
</script>

<template>
    <div v-if="type.id == task.taskType" class="task-wrapper">
        <div draggable="true" @dragstart="taskDrag(task.id)">
            <div>Task -  {{ task.title }}</div>
            <div>Description - {{ task.description }}</div>
            <div>Estimated Time - {{ new Date(task.estimatedTime) }}</div>
            <div>Attachments - {{ task.files.length }}</div>
            <ul>
                <li v-for="(file, index) in task.files" :key="index">{{file.name}}</li>
            </ul>
            <span class="edit-task" @click="editTask(task.id)">Edit</span>
        </div>
    </div>
</template>

<style>

.task-wrapper {
  padding: 5px;
  margin-bottom: 10px;
  background: #d3d3d3;
}
.edit-task {
    text-decoration: underline;
    color: #089720;
    font-weight: 700;
    cursor: pointer;
}
</style>
