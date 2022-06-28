<script setup>
import Task from '@/components/Task.vue';
import Modal from '@/components/Modal.vue';
import Phrase from '@/components/Phrase.vue';
import Percentage from '@/components/Progress.vue';
</script>

<template>
  <header>
    <h1 class="app-title">Task Planner</h1>
    <Percentage :task-list="taskList"/>
    <div class="btn-wrapper">
        <button class="btn primary align-right" type="button" @click="showModal">Create Task</button>
    </div>
  </header>
  <div class="board-wrapper">
    <div class="board" v-for="type in taskTypes" :key="type.id">
      <div class="board-container" :id="type.id" @drop="taskDrop" @dragover="taskAllowDrop" >
        <h1>{{type.name}} - <Phrase :id="type.id" :task-list="taskList" /></h1>
          <template v-for="task in taskList" :key="task.title">
            <Task :task="task" :type="type" @drag-task="dragTask" @edit-task="setEditTask"/>
          </template>
      </div>
    </div>
  </div>
  <div v-if="isShowModal" class="modal-overlay">
    <Modal :taskTypes="taskTypes" :editTask="editTask" @add-task="addTask" @hide-modal="hideModal" />
  </div>
</template>

<script>
export default {
  // state
  data() {
    return {
      isShowModal: false,
      count: 0,
      taskTypes: [
        {
          name: "Pending",
          id: "1"
        },
        {
          name: "Processing",
          id: "2"
        },
        {
          name:"Done",
          id: "3"
        }
      ],
      taskList: [
        {
          id: 1,
          description: "vuejs learning",
          estimatedTime: 1656553680000,
          taskType: "1",
          title: "learning",
          files: [
            {
              name: "IRF.pdf",
              size: 7852257,
              type: "application/pdf"
            }
          ]
        }
      ],
      dragTaskId:0,
      editTask: {}
    }
  },
  // actions
  methods: {
    showModal() {
      this.isShowModal = true;
    },
    hideModal() {
      this.isShowModal = false;
      this.editTask = {};
    },
    addTask(newTask) {
      newTask.estimatedTime = new Date(newTask.estimatedTime).getTime();
      newTask.id = this.taskList.length > 0 ? this.taskList[this.taskList.length-1].id+1 : 1;
      this.taskList.push(newTask);
      this.isShowModal = false;
      console.log(this.taskList)
    },
    setEditTask(id) {
      this.isShowModal = true;
      const task = this.taskList.find(item => item.id == id);
      const dateObject = new Date(task.estimatedTime);
      //task.estimatedTime= new Date(dateObject.getTime() + new Date().getTimezoneOffset() * -60 * 1000).toISOString().slice(0, 19)
      this.editTask = {...task};
    },
    taskDrop(e) {
      this.taskList.forEach(item => {
        if(item.id === this.dragTaskId && e.target.id) {
          item.taskType = Number(e.target.id);
        }
      });
      e.preventDefault();
    },
    taskAllowDrop(e) {
      e.preventDefault();
    },
    dragTask(id) {
      this.dragTaskId = id;
    }
  }
}
</script>

<style>
@import '@/assets/base.css';

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  font-weight: normal;
}

.board-wrapper {
  display: flex;
  flex-wrap: wrap;
}

.board {
  flex-grow: 1;
  padding: 10px;
  width: 13%;
}

.board-container {
  border: 1px solid #ccc;
  padding: 5px;
  background: #f0f0f0;
  border-radius: 5px;
  min-height: 200px;
}

.board-container h1 {
  text-align: center;
  font-size: 18px;
  font-weight: 700;
}

.btn-wrapper {
  margin-top: 20px;
  text-align: right;
}

.btn {
  padding: 5px 15px;
  margin-right: 10px;
  cursor: pointer;
}

.primary {
  background: #089720;
  color: #fff;
  border: 1px solid #046e16;
}

.secondary {
  background: #8f0505;
  color: #fff;
  border: 1px solid #570404;
}

.app-title {
  font-weight: 700;
  text-align: center;
  margin-bottom: 20px
}
</style>
