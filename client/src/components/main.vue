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
        <form class="ui form" v-on:submit.prevent="addTask">
          <div class="field">
            <div class="fields">
              <div class="eight wide field">
                <label>Title</label>
                <input type="text" name="title" placeholder="input task title" v-model="newTask.title">
              </div>
              <div class="eight wide field">
                <label>Assign To</label>
                <input type="text" name="asignTo" value="" placeholder="asign into.." v-model="newTask.asignTo">
              </div>
            </div>
          </div>
          <div class="field">
            <label>Description</label>
            <textarea name="Description" placeholder="input decription of task" v-model="newTask.description"></textarea>
          </div>
          <div class="two fields">
            <div class="nine wide field">
              <label>Point</label>
              <input type="text" name="point" placeholder="input your task point" v-model="newTask.point">
            </div>
            <div class="seven wide field">
              <label>Status</label>
              <select class="ui fluid dropdown" name="status" v-on:click="setDropDown" v-model="newTask.status">
                <option value="back-log">Back-Log</option>
                <option value="to-do">To-Do</option>
                <option value="doing">Doing</option>
                <option value="done">Done</option>
              </select>
            </div>
          </div>
          <div class="actions">
            <div class="ui black deny button">
              Cancel
            </div>
            <button type="submit" name="prev" class="ui labeled icon button"><i class="save icon"></i>Save</button>
          </div>
        </form>
      </div>
    </div>
    <div class="maincontainer" style="padding-top: 10px; padding-left: 15px; padding-right: 15px;">
      <div class="ui container">
        <h3 class="ui horizontal divider header">
          <i class="check circle icon"></i>
          DONE
        </h3>
      </div>
      <div class="ui container" style="padding-top:20px;">
        <div class="ui link cards">
          <div class="card" v-for="task in tasks" v-if="task.status == 'done'">
            <div class="content">
              <div class="header">{{ task.title }}</div>
            </div>
            <div class="content">
              <h4 class="ui sub header">Activity</h4>
              <div class="ui small feed">
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Sign To :</a> {{ task.asignTo }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Description :</a> {{ task.description }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Point :</a> {{ task.point }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>status :</a> {{ task.status }}
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="extra content">
              <button class="ui animated button" v-on:click="deleteTask(task)">
                <div class="visible content">
                  Delete
                </div>
                <div class="hidden content">
                  <i class="trash outline icon"></i>
                </div>
              </button>
              <button class="ui animated button" v-on:click="updateTask(task, 'doing')">
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
    <div class="maincontainer" style="padding-top: 10px; padding-left: 15px; padding-right: 15px;">
      <div class="ui container">
        <h3 class="ui horizontal divider header">
          <i class="street view icon"></i>
          DOING
        </h3>
      </div>
      <div class="ui container" style="padding-top:20px;">
        <div class="ui link cards">
          <div class="card" v-for="task in tasks" v-if="task.status == 'doing'">
            <div class="content">
              <div class="header">{{ task.title }}</div>
            </div>
            <div class="content">
              <h4 class="ui sub header">Activity</h4>
              <div class="ui small feed">
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Sign To :</a> {{ task.asignTo }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Description :</a> {{ task.description }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Point :</a> {{ task.point }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>status :</a> {{ task.status }}
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="extra content">
              <button class="ui animated button" v-on:click="deleteTask(task)">
                <div class="visible content">
                  Delete
                </div>
                <div class="hidden content">
                  <i class="trash outline icon"></i>
                </div>
              </button>
              <button class="ui animated button" v-on:click="updateTask(task, 'to-do')">
                <div class="visible content">
                  To-do
                </div>
                <div class="hidden content">
                  <i class="arrow down icon"></i>
                </div>
              </button>
              <button class="ui animated button" v-on:click="updateTask(task, 'done')">
                <div class="visible content">
                  Done
                </div>
                <div class="hidden content">
                  <i class="arrow up icon"></i>
                </div>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="maincontainer" style="padding-top: 10px; padding-left: 15px; padding-right: 15px;">
      <div class="ui container">
        <h3 class="ui horizontal divider header">
          <i class="book icon"></i>
          TO-DO
        </h3>
      </div>
      <div class="ui container" style="padding-top:20px;">
        <div class="ui link cards">
          <div class="card" v-for="task in tasks" v-if="task.status == 'to-do'">
            <div class="content">
              <div class="header">{{ task.title }}</div>
            </div>
            <div class="content">
              <h4 class="ui sub header">Activity</h4>
              <div class="ui small feed">
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Sign To :</a> {{ task.asignTo }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Description :</a> {{ task.description }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Point :</a> {{ task.point }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>status :</a> {{ task.status }}
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="extra content">
              <button class="ui animated button" v-on:click="deleteTask(task)">
                <div class="visible content">
                  Delete
                </div>
                <div class="hidden content">
                  <i class="trash outline icon"></i>
                </div>
              </button>
              <button class="ui animated button" v-on:click="updateTask(task, 'back-log')">
                <div class="visible content">
                  Back
                </div>
                <div class="hidden content">
                  <i class="arrow down icon"></i>
                </div>
              </button>
              <button class="ui animated button" v-on:click="updateTask(task, 'doing')">
                <div class="visible content">
                  Doing
                </div>
                <div class="hidden content">
                  <i class="arrow up icon"></i>
                </div>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="maincontainer" style="padding-top: 10px; padding-left: 15px; padding-right: 15px;">
      <div class="ui container">
        <h3 class="ui horizontal divider header">
          <i class="bookmark icon"></i>
          BACK-LOG
        </h3>
      </div>
      <div class="ui container" style="padding-top:20px;">
        <div class="ui link cards">
          <div class="card" v-for="task in tasks" v-if="task.status == 'back-log'">
            <div class="content">
              <div class="header">{{ task.title }}</div>
            </div>
            <div class="content">
              <h4 class="ui sub header">Activity</h4>
              <div class="ui small feed">
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Sign To :</a> {{ task.asignTo }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Description :</a> {{ task.description }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>Point :</a> {{ task.point }}
                    </div>
                  </div>
                </div>
                <div class="event">
                  <div class="content">
                    <div class="summary">
                       <a>status :</a> {{ task.status }}
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="extra content">
              <button class="ui animated button" v-on:click="deleteTask(task)">
                <div class="visible content">
                  Delete
                </div>
                <div class="hidden content">
                  <i class="trash outline icon"></i>
                </div>
              </button>
              <button class="ui animated button" v-on:click="updateTask(task, 'to-do')">
                <div class="visible content">
                  To-do
                </div>
                <div class="hidden content">
                  <i class="arrow up icon"></i>
                </div>
              </button>
            </div>
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
  const taskRef = db.ref('tasks')
  export default {
    data(){
      return{
        newTask: {
          title: '',
          asignTo: '',
          description: '',
          point: 0,
          status: '' || 'back-log'
        }
      }
    },
      firebase: {
        tasks: taskRef
      },
      methods: {
        openModal(){
          $('.ui.modal')
            .modal('setting', 'transition', 'vertical flip')
            .modal('show');
        },
        setDropDown(){
          $('select.dropdown')
            .dropdown();
        },
    		deleteTask(task){
    	     taskRef.child(task['.key']).remove();
    		},
        updateTask(task, status){
          //let this = self;
          let statusBaru = status
          console.log(task);
          let newTask = {
            title : task.title,
            asignTo : task.asignTo,
            description : task.description,
            point : task.point,
            status : statusBaru
          }
          taskRef.child(task['.key']).remove()
          taskRef.child(task['.key']).set(newTask)
        },
    		addTask(){
          taskRef.push(this.newTask);
          this.newTask.title = '';
          this.newTask.asignTo = '';
          this.newTask.point = 0;
          this.newTask.description = '';
          this.newTask.status = '';
          alert('Add Task Success');
        },

    		asyncCancel(){
    			this.title = ''
    			this.description =''
    			this.point = 0
    			this.assignTo = ''
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
