<script>
export default {
    props: {
        taskList: Object,
        id: String
    },
    computed: {
        printPhrase() {
        const lengthOfTask = this.taskList.filter(task => task.taskType === this.id);
        const percentage = ( lengthOfTask.length / this.taskList.length ) * 100;
        return `${lengthOfTask.length} / ${this.taskList.length} (${percentage}%)`;
        }
    }
}
</script>

<!-- view -->
<template>
    {{printPhrase}}
</template>