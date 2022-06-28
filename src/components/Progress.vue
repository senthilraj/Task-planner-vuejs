<script>
export default {
    props: {
        taskList: Object
    },
    computed: {
        taskProgress() {
            const lengthOfTask = this.taskList.filter(task => task.taskType == 3);
            const percentage = ( lengthOfTask.length / this.taskList.length ) * 100;
            const phrase = `${percentage}% (Done ${lengthOfTask.length} out of ${this.taskList.length})`;
            return { percentage, phrase };
        }
    }
}
</script>

<!-- view -->
<template>
    <div class="progress-bar-wrapper">
        <h1 class="progress-bar-title">Progess</h1> - <span>{{taskProgress.phrase}}</span>
        <div class="progress-bar-container">
            <div class="progress-bar-percentage" :style="{ width: taskProgress.percentage+'%'}"></div>
        </div>
    </div>
</template>


<style>
.progress-bar-wrapper {
    padding: 10px;
    background: #f0f0f0;
    border-radius: 5px;
}
.progress-bar-title {
    font-size: 18px;
    font-weight: 700;
    display: inline-block;
}
.progress-bar-container {
    position: relative;
    height: 10px;
    width: 100%;
    background: #90ca9a;
    border-radius: 10px;
}

.progress-bar-percentage {
    border-radius: 10px 0 0 10px;
    position: absolute;
    left: 0;
    top:0;
    width: 0%;
    height: 10px;
    background: #089720;
}
</style>