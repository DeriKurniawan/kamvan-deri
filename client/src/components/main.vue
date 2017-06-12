<template lang="html">
  <div>
    <nav>
      <div class="ui large stackable top fixed menu">
        <div class="item">
          <i class="file text icon"></i>
          <b>Kan-Ban Tasking Task</b>
        </div>
        <div class="right menu">
          <div class="item">
            <button type="button" class="ui labeled icon primary button" v-on:click="openModal"><i class="add icon"></i>Add Task</button>
          </div>
        </div>
      </div>
    </nav>
    <div class="ui modal">
      <div class="header">
        Create New task
      </div>
      <div class="content">
        <form class="ui form">
          <div class="field">
            <div class="fields">
              <div class="eight wide field">
                <label>Title</label>
                <input type="text" name="title" placeholder="input task">
              </div>
              <div class="eight wide field">
                <label>Assign To</label>
                <input type="text" name="assignTo" value="" placeholder="assign into..">
              </div>
            </div>
          </div>
          <div class="field">
            <label>Description</label>
            <textarea name="Description"></textarea>
          </div>
          <div class="two fields">
            <div class="nine wide field">
              <label>Point</label>
              <input type="text" name="point" placeholder="input your task point">
            </div>
            <div class="seven wide field">
              <label>Status</label>
              <select class="ui fluid dropdown" name="" v-on:click="setDropDown">
                <option value="backlog">Back-Log</option>
                <option value="todo">To-Do</option>
                <option value="doing">Doing</option>
                <option value="done">Done</option>
              </select>
            </div>
          </div>
          <div class="actions">
            <div class="ui black deny button">
              Cancel
            </div>
            <button type="button" name="prev" class="ui labeled icon button" v-on:click="asyncSave"><i class="save icon"></i>Save</button>
          </div>
        </form>
      </div>
    </div>
    <div class="maincontainer" style="padding-top: 10px; padding-left: 15px; padding-right: 15px;">
      <div class="ui fluid container">
        <h3 class="ui horizontal divider header">
          <i class="checkmark box icon"></i>
          DONE
        </h3>
      </div>
      <div class="ui fluid container">
        <div class="ui card" v-for="">
          <div class="content">
            <div class="header">Project Title</div>
          </div>
          <div class="content">
            <h4 class="ui sub header">Activity</h4>
            <div class="ui small feed">
              <div class="event">
                <div class="content">
                  <div class="summary">
                     <a>Sign To :</a> name of assignTo
                  </div>
                </div>
              </div>
              <div class="event">
                <div class="content">
                  <div class="summary">
                     <a>Description :</a> text of description
                  </div>
                </div>
              </div>
              <div class="event">
                <div class="content">
                  <div class="summary">
                     <a>Point :</a> added point for yout task
                  </div>
                </div>
              </div>
              <div class="event">
                <div class="content">
                  <div class="summary">
                     <a>status :</a> added point for yout task
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="extra content">
            <button class="ui animated button">
              <div class="visible content">
                Delete
              </div>
              <div class="hidden content">
                <i class="trash outline icon"></i>
              </div>
            </button>
            <button class="ui animated button">
              <div class="visible content">
                Doing
              </div>
              <div class="hidden content">
                <i class="arrow down icon"></i>
              </div>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import * as firebase from 'firebase'
  var config = {
    apiKey: "AIzaSyB-Pv68tXh6_EILD5DgsxWP7YHipMr_tOA",
    authDomain: "kanban-1fa99.firebaseapp.com",
    databaseURL: "https://kanban-1fa99.firebaseio.com",
    projectId: "kanban-1fa99",
    storageBucket: "kanban-1fa99.appspot.com",
    messagingSenderId: "538961608958"
  };
  //firebase.initializeApp(config);
  const firebaseApp = firebase.initializeApp(config);
  const db = firebaseApp.database()
  const taskRef = db.ref('task')
  export default {
    data(){
      return{
        showNewDialogTask: false,
        title: '',
        description: '',
        point: 0,
        assignTo: '',
        status: 0,
        loding: true,
        task: {
          taskId : null,
  				title : null,
  				description :null,
  				point : null,
  				assignedTo : null,
  				status: null
        },
        taskNext: '',
        taskPrev: '',
        showModalSticky: false
      }
    },
      firebase: {
        tasks: taskRef
      },
      methods: {
        showModal(task){
          this.task = task,
          showModalSticky = true,
          this.next(),
          this.prev()
        },
        openModal(){
          $('.ui.modal')
            .modal('setting', 'transition', 'vertical flip')
            .modal('show');
        },
        setDropDown(){
          $('select.dropdown')
            .dropdown();
        },
        next(){
          var self = this
    			switch(self.task.status){
    				case 0:
    				self.textNext = 'To Do'
    				break;
    				case 1:
    				self.textNext = 'Doing'
    				break;
    				case 2:
    				self.textNext = 'Done'
    				break;
    				case 3:
    				self.textNext = 'Done'
    				break;
    			}
        },
        prev(){
    			var self = this
    			switch(self.task.status){
    				case 0:
    				self.textPrev = 'Back Log'
    				break;
    				case 1:
    				self.textPrev = 'Back Log'
    				break;
    				case 2:
    				self.textPrev = 'To Do'
    				break;
    				case 3:
    				self.textPrev = 'Doing'
    				break;
    			}
    		},
    		updateNext(task, newStatus){
    			tasksRef.child(task['.key'])
    			.child('status')
    			.set(newStatus)
    			this.showModalSticky = false
    		},
    		updatePrev(task, newStatus){
    			tasksRef.child(task['.key'])
    			.child('status')
    			.set(newStatus)
    			this.showModalSticky = false
    		},
    		deleteTask(task){
    			this.$firebaseRefs.tasks.child(task['.key']).remove()
    			this.showModalSticky = false
    		},
    		asyncSave(){
    			var self = this
    			let str = '0123456789';
    			let length = 3;
    			let result = '';
    			for (let i = length; i > 0; i--) {
    				result += str[Math.floor(Math.random() * str.length)];
    			}
    			var data = {
    				taskId : result,
    				title: self.title,
    				description: self.description,
    				point: self.point,
    				assignedTo : self.assignedTo,
    				status : self.status
    			}
    			tasksRef.push(data)
    			setTimeout(() => {
    				self.loading = false;
    				self.showModalNewTask = false
    				self.title = ''
    				self.description =''
    				self.point = 0
    				self.assignedTo = ''
    			}, 2000);
    		},
    		asyncCancel(){
    			this.showModal = false
    			this.title = ''
    			this.description =''
    			this.point = 0
    			this.assignedTo = ''
    		}
    	}
    }
</script>
<style lang="css">
  #app: {
    padding: 0px;
  }

  .maincontainer: {
    padding-top: 0px;
  }
</style>
