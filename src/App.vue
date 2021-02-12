<template>
  <div>
    <main class="main">
      <form class="task" action="#" method="post">
        <input class="task__description task__name" type="text" placeholder="Insert your task name" maxlength="30" required
          v-bind:value="newTaskName"
          v-on:input="newTaskName = $event.target.value"
          v-on:keydown.enter.prevent="addNewTask()">

        <span class="task__counter">
            {{newTaskName.length}}/30
        </span>

        <textarea class="task__description" name="task" id="task" placeholder="Describe your task for today"
          v-bind:value="newTaskDescription"
          v-on:input="newTaskDescription = $event.target.value"></textarea>

        <label class="task__priority-label" for="priority">Priority: </label>

        <select class="task__choose_priority" name="priority" id="priority"
          v-bind="newTaskPriority"
          v-on:change="newTaskPriority = $event.target.value">

          <option disabled value="" selected>Choose priority</option>
          <option value="1">High</option>
          <option value="2">Medium</option>
          <option value="3">Low</option>
        </select>

        <section class="buttons">
            <button class="buttons__button button button--sort" title="Sort from high priority to low" v-on:click.prevent="sortTaskList()" > Sort </button>
            <button class="buttons__button button button--add" title="Click to add your new task" v-on:click.prevent="addNewTask()"> Add </button>
        </section>

      </form>

      <ul v-show="getLength" class="existing_tasks">
        <singleTask
          v-for="task of tasks" :key="task"
          v-bind:task="task"
          v-on:deleteItem="deleteTask(task.id)"
          />
      </ul>

      <p class="no-task" v-show="!getLength">
        Please add your first task for today
      </p>

    </main>
  </div>
</template>

<script>

import singleTask from "./taskListItem.vue";

  export default {
  components: {
    singleTask
  },

    methods: {

      addNewTask() {
        let newTask = Object.assign(
          {},
          this.blankTask(),
          {
            name: this.newTaskName,
            description: this.newTaskDescription,
            priority: this.newTaskPriority
          });

        this.tasks.push(newTask);

        this.newTaskName = "";
        this.newTaskDescription = "";
      },

      sortTaskList() {
        if (this.prevTasksList.length < 1) {
          this.prevTasksList = this.tasks.slice();
          this.tasks.sort((a, b) => a.priority - b.priority);
          return;
        }

        this.tasks = this.prevTasksList;
        this.prevTasksList = [];
      },

      deleteTask(element) {
        this.tasks = this.tasks.filter((task) => Number(task.id) !== element)
      }
    },

    computed: {
      getLength() {
        return this.tasks.length;
      },
    },

    data() {
      return {
        newTaskName:"",
        newTaskDescription: "",
        newTaskPriority: 3,

        prevTasksList: [],

        tasks: [
          {
            id: 1,
            name: "Fitter happier",
            description: "More productive Comfortable Not drinking too much",
            isDone: false,
            priority: 1,
            isAddjusted: false
          },
          {
            id: 2,
            name: "Regular exercise",
            description: "Regular exercise at the gym (3 days a week)",
            isDone: false,
            priority: 2,
            isAddjusted: false
          },
          {
            id: 3,
            name: "Keep in contact",
            description: "Keep in contact with old friends (enjoy a drink now and then)",
            isDone: false,
            priority: 3,
            isAddjusted: false
          },
          {
            id: 4,
            name: "Check credit",
            description: "Will frequently check credit at (moral) bank (hole in the wall)",
            isDone: true,
            priority: 1,
            isAddjusted: false
          },
          {
            id: 5,
            name: "Fitter, healthier and more productive",
            description: "A pig In a cage On antibiotics",
            isDone: true,
            priority: 1,
            isAddjusted: false
          }
        ],

        blankTask:() => {
          return {
            id: Math.round(Math.random(0, 1) * 1000),
            name: "",
            description: "",
            isDone: false,
            priority: 3,
            isAddjusted: false
          }
        }
      }
    }
  }
</script>

<style>
  .main {
    font-family: Arial, Helvetica, sans-serif;
    margin-left: auto;
    margin-right: auto;
    width: 420px;
    min-height: 1000px;
    padding-top: 40px;
    padding-left: 40px;
    border: black solid 1px;
    border-radius: 5px;
    box-sizing: border-box;
  }

  .task {
    position: relative;
  }

  .task__description {
    box-sizing: border-box;
    width: 330px;
    height: 99px;
    resize: none;
    padding-left: 10px;
    padding-top: 10px;
  }

  .task__name {
    height: 30px;
    font-size: 16px;
    line-height: 20px;
    padding-bottom: 10px;
    color: grey;
    margin-bottom: 5px;
  }

  .task__counter {
    position: absolute;
    top: 10px;
    right: 55px;
    font-size: 10px;
    color: grey;
  }

  .task__priority-label {
    display: inline-block;
    font-size: 15px;
    font-weight: 400;
    padding-right: 25px;
    color: tomato;
  }

  .task__choose_priority {
    display: inline-block;
    width: 250px;
    margin-bottom: 20px;
    margin-top: 0;
  }

  .buttons {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    width: 330px;
  }

  .button {
    font-size: 12px;
    outline: none;
    border-style: solid;
    border-color: rgb(122, 81, 81);
    border-width: 1px;
    width: 240px;
    height: 25px;
    margin-bottom: 20px;
    border-radius: 5px;
  }

  .button--add:hover {
    background-color: grey;
    color: ivory;
  }

  .button--add:focus {
    background-color: grey;
    color: ivory;
  }

  .button--add:active {
    background-color: rgba(0, 0, 0, 0.6);
    color: ivory;
  }

  .button--sort {
    width: 65px;
    color: grey;
    background-color: white;
  }

  .button--sort:hover {
    background-color: grey;
    color: ivory;
  }

  .button--sort:focus {
    background-color: grey;
    color: ivory;
  }

  .button--sort:active {
    background-color: rgba(0, 0, 0, 0.6);
    color: ivory;
  }

  .existing_tasks {
    list-style: none;
    padding: 0;
  }

  .no-task {
    color: gainsboro;
    font-size: 23px;
    margin-top: 50px;
  }

</style>
