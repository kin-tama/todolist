<template>
  <div>
    <header v-show="!getLength">
        <button>
            add new task
        </button>
    </header>
    <main class="main">
      <form class="main__form" action="#" method="post">
        <input class="main__task main__task--name" type="text" placeholder="Insert your task name" maxlength="30" required
          v-bind:value="newTaskName"
          v-on:input="newTaskName = $event.target.value"
          v-on:keydown.enter.prevent="addNewTask()"
          >
        <span class="counter">
            {{newTaskName.length}}/30
        </span>

        <textarea class="main__task" name="task" id="task" placeholder="Describe your task for today"
        v-bind:value="newTaskDescription"
        v-on:input="newTaskDescription = $event.target.value"></textarea>

        <label class="priority-label" for="priority">Priority: </label>

        <select class="choose_priority" name="priority" id="priority"
          v-bind="newTaskPriority"
          v-on:change="newTaskPriority = $event.target.value">

          <option disabled value="" selected>Choose priority</option>
          <option value="1">High</option>
          <option value="2">Medium</option>
          <option value="3">Low</option>
        </select>

        <section class="buttons">
            <button class="button button--sort" title="Sort from high priority to low" v-on:click.prevent="sortTaskList()" > Sort </button>
            <button class="button button--add" title="Click to add your new task" v-on:click.prevent="addNewTask()"> Add </button>
        </section>

      </form>

      <ul class="exist_tasks">
        <li class="exist_task"
            v-for="task in tasks"
        >
        <div class="exist_task__wrapper">
            <p class="priority"> {{ PRIORITIES[task.priority] }}</p>
            <p class="task__name" v-bind:class="{'task__name--done':task.isDone}"> {{ task.name }}</p>
            <button class="button button--done" v-on:click="task.isDone = !task.isDone"></button>
            <button class="button button--delete" :id="task.id" v-on:click="deleteTask(task.id)"></button>
            <button class="button button--expand" v-on:click="task.isExpanded = !task.isExpanded"></button>
        </div>
        <p class="task__description" v-if="task.isExpanded">{{ task.description }} </p>
        </li>

      </ul>

    </main>
  </div>
</template>


<script>
  export default {
    methods: {

      addNewTask() {
        let newTask = Object.assign(
          {},
          this.blankTask,
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

      deleteTask(some) {
        this.tasks = this.tasks.filter((task) => Number(task.id) !== some)
      }
    },

    computed: {

      getLength() {
        return this.tasks.length;
      },

      getStringLength(string) {
        return string.length;
      }

    },

    data() {
      return {
        PRIORITIES: {
          1: "High",
          2: "Middle",
          3: "Low"
        },

        newTaskName:"",
        newTaskDescription: "",
        newTaskPriority: 3,

        prevTasksList: [],

        tasks: [
          {
            id: 1,
            name: "Buy some bread",
            description: "You have no food at home.",
            isDone: false,
            priority: 1,
            isExpanded: false
          },
          {
            id: 2,
            name: "Feed your cat",
            description: "Your cat is hungry.",
            isDone: false,
            priority: 2,
            isExpanded: false
          },
          {
            id: 3,
            name: "Go to bed",
            description: "Your bed is empty. You haven't slept for ages.",
            isDone: false,
            priority: 3,
            isExpanded: false
          },
          {
            id: 4,
            name: "Take your pills",
            description: "You are seriously sick. Don't forget to take your meds.",
            isDone: true,
            priority: 1,
            isExpanded: false
          },
          {
            id: 5,
            name: "Call someone",
            description: "You need help. Call the police",
            isDone: true,
            priority: 1,
            isExpanded: false
          }
        ],

        blankTask: {
          id: (Math.random(0, 1) * 1000),
          name: "",
          description: "",
          isDone: false,
          priority: 3,
          isExpanded: false
        }
      }
    }
  }
</script>


<style>
  .main {
    margin-left: auto;
    margin-right: auto;
    font-family: Arial, Helvetica, sans-serif;
    width: 420px;
    min-height: 1000px;
    padding-top: 40px;
    padding-left: 40px;
    border: black solid 1px;
    border-radius: 5px;
    box-sizing: border-box;
  }

  .main__form {
    position: relative;
  }

  .main__task {
    box-sizing: border-box;
    width: 330px;
    height: 99px;
    resize: none;
    padding-left: 10px;
    padding-top: 10px;
  }

  .main__task--name {
    height: 30px;
    font-size: 16px;
    line-height: 20px;
    padding-bottom: 10px;
    color:cadetblue;
    margin-bottom: 5px;
  }

  .counter {
    position: absolute;
    top: 10px;
    right: 55px;
    font-size: 10px;
    color: grey;
  }

  .priority-label {
    display: inline-block;
    font-family: Arial, Helvetica, sans-serif;
    font-size: 15px;
    font-weight: 400;
    padding-right: 25px;
    color: #3078b4;
  }

  .choose_priority {
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
    font-family: Arial, Helvetica, sans-serif;
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

  .button--delete {
    height: 20px;
    width: 20px;
    position: absolute;
    bottom: -10px;
    right: 5px;
    padding: 0;
    width: 20px;
    border-radius: 50%;
    border: none;
    background-image: url("./img/del.svg");
    background-repeat: no-repeat;
    background-position-x: 5px;
    background-position-y: 5px;
    background-color: white;
  }

  .button--delete:hover {
    background-color: rgba(0, 0, 0, 0.1);
  }

  .button--delete:focus {
    background-color: rgba(0, 0, 0, 0.1);
  }

  .button--delete:active {
    background-color: rgba(0, 0, 0, 0.3);
  }

  .button--done {
    height: 20px;
    width: 20px;
    position: absolute;
    bottom: -10px;
    right: 30px;
    padding: 0;
    border: none;
    background-image: url("./img/ok.svg");
    background-repeat: no-repeat;
    background-position-x: 5px;
    background-position-y: 5px;
    background-color: white;
    border-radius: 50%;
  }

  .button--done:hover {
    background-color: rgba(0, 0, 0, 0.1);
  }

  .button--done:focus {
    background-color: rgba(0, 0, 0, 0.1);
  }

  .button--done:active {
    background-color: rgba(0, 0, 0, 0.3);
  }

  .button--expand {
    height: 15px;
    width: 15px;
    position: absolute;
    bottom: 5px;
    right: -15px;
    padding: 0;
    border: none;
    background-image: url("./img/expand.svg");
    background-size: 10px;
    background-repeat: no-repeat;
    background-position-x: 2px;
    background-position-y: 2px;
    background-color: ghostwhite;
    border-radius: 2px;
  }

  .button--expand:hover {
    background-color: rgba(0, 0, 0, 0.1);
  }

  .button--expand:focus {
    background-color: rgba(0, 0, 0, 0.1);
  }

  .button--expand:active {
    background-color: rgba(0, 0, 0, 0.3);
  }

  .exist_tasks {
    list-style: none;
    padding: 0;
  }

  .exist_task {
    display: flex;
    flex-direction: column;
    width: 330px;
  }

  .exist_task__wrapper {
    position: relative;
    display: flex;
    flex-direction: row;
    align-items: center;
  }

  .task__name {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 12px;
    line-height: 20px;
    padding-top: 10px;
    padding-bottom: 0;
    padding-left: 10px;
    vertical-align: middle;
    width: 266px;
    height: 30px;
    background-color:ghostwhite;
    border-radius: 5px;
    margin-bottom: 0;
  }

  .task__name--done {
    color: green;
    text-decoration: line-through;
  }

  .task__description {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 12px;
    line-height: 20px;
    padding-top: 10px;
    padding-bottom: 10px;
    padding-left: 10px;
    vertical-align: middle;
    width: 330px;
    border: grey solid 1px;
    border-radius: 5px;
    margin-top: 0;
  }

  .priority {
    font-size: 12px;
    line-height: 20px;
    width: 60px;
  }
</style>
