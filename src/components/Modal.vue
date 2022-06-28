<script>
    export default {
        props: {
            taskTypes: Object,
            editTask: Object
        },
        created() {
            if(this.editTask && Object.keys(this.editTask).length) {
                this.newTask = this.editTask;
            }
        },
        data() {
            return {
                newTask: {
                    title:"",
                    description: "",
                    attachments: [],
                    estimatedTime: "",
                    taskType: 0,
                    files: []
                }
            }
        },
        methods: {
            changeLocation(e) {
                console.log(e.target.value);
                this.newTask.taskType = e.target.value;
            },
            addTask() {
                this.$emit('addTask', this.newTask);
                this.newTask = {
                    title:"",
                    description: "",
                    attachments: [],
                    estimatedTime: "",
                    taskType: 0,
                    files: []
                };
            },
            hideModal() {
                this.$emit('hideModal');
                this.newTask = {
                    title:"",
                    description: "",
                    attachments: [],
                    estimatedTime: "",
                    taskType: 0,
                    files: []
                };
            },
             previewFiles() {
                this.newTask.files = [...this.newTask.files, ...this.$refs.myFiles.files];
            },
            removeFiles(id) {
                this.newTask.files.splice(id, 1);
            }
        }
    }
</script>

<template>
    <div class="modal">
        <div class="modal-title">{{this.editTask && Object.keys(this.editTask).length ? "Edit" : "Create"}} Task</div>
        <div class="modal-content">
        <div class="form-group">
            <label>Title</label>
            <input type="text" v-model="newTask.title" />
        </div>
        <div class="form-group">
            <label>Description</label>
            <textarea v-model="newTask.description" />
        </div>
        <div class="form-group">
            <label>Estimated Time</label>
            <input type="datetime-local" v-bind="newTask.estimatedTime" />
        </div>
        <div class="form-group">
            <label>Progress</label>
            <select :required="true" @change="changeLocation">
                <option :selected="true">Select status</option>
                <option v-for="type in taskTypes" v-bind:value="type.id" :selected="type.id == newTask.taskType">{{ type.name }}</option>
            </select>
        </div>
        <div class="form-group">
            <input type="file" id="file" ref="myFiles" class="custom-file-input" @change="previewFiles" multiple/>
            <ul class="files-preview">
                <li v-for="(file, index) in newTask.files" :key="index">{{file.name}} <span class="remove-file" @click="removeFiles(index)">x</span></li>
            </ul>
        </div>
        <div class="form-group">
            <button class="btn primary" type="button" @click="addTask">Submit</button>
            <button class="btn secondary" type="button" @click="hideModal">Cancel</button>
        </div>
        </div>
    </div>
</template>

<style>
.modal-overlay {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  background: rgb(0 0 0 / 40%);
  overflow: auto;
}

.modal {
  position: relative;
  width: 500px;
  margin-left: -250px;
  left: 50%;
  top: 50px;
  background: #fff;
  padding: 20px;
}

.modal-title {
    text-align: center;
    font-weight: 700;
}

.modal-content {
    margin-top: 10px;
}

.form-group {
    margin-bottom: 10px;
}

.form-group label {
    font-size: 14px;
    font-weight: 700;
    width: 100%;
    display: inline-block;
    margin-bottom: 5px;
}

.form-group input[type="text"], .form-group select, .form-group textarea {
    width: 100%;
    min-height: 30px;
}

.remove-file {
    cursor: pointer;
    font-weight: 700;
    float: right;
}

.files-preview {
    width: 200px;
}

</style>