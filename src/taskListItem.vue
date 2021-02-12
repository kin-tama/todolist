<template>
  <li class="existing_tasks"
    v-on:keydown.esc="task.isAddjusted = false,
      task.isExpanded = false">
    <div class="existing_task__wrapper">
    <p class="existing_task__priority"
      v-if="!task.isAddjusted"
      v-bind:class="[
        {'existing_task__priority--high': task.priority == 1},
        {'existing_task__priority--medium': task.priority == 2},
        {'existing_task__priority--low': task.priority == 3}
      ]"
      v-on:click="task.isAddjusted = true"
    > {{ PRIORITIES[task.priority] }} </p>

    <select class="existing_task__priority" name="priority" id="priority"
      v-if="task.isAddjusted"
      v-on:change="task.priority = $event.target.value"
      v-bind:class="[
        {'existing_task__priority--high': task.priority == 1},
        {'existing_task__priority--medium': task.priority == 2},
        {'existing_task__priority--low': task.priority == 3}
      ]">

      <option value="1" v-bind:selected="task.priority == 1">High</option>
      <option value="2" v-bind:selected="task.priority == 2">Medium</option>
      <option value="3" v-bind:selected="task.priority == 3">Low</option>

    </select>

      <p class="existing_task__name"
        v-if="!task.isAddjusted"
        v-bind:class="{'task__name--done':task.isDone}"
        v-on:click="task.isAddjusted = true"
      > {{ task.name }} </p>

      <input class="existing_task__name existing_task__name--adjusted"
        v-if="task.isAddjusted"
        v-bind:value="task.name"
        v-on:input="task.name = $event.target.value"
      >

      <button class="button button--done"
        v-on:click="task.isDone = !task.isDone"
        v-bind:disabled="task.isAddjusted"
      >
      </button>

      <button class="button button--delete" :id="task.id" v-on:click="$emit('deleteItem', task.id)">
      </button>
      <button class="button button--expand"
        v-bind:class="{'button--shrink': task.isAddjusted}"
        v-on:click="task.isAddjusted = !task.isAddjusted; $emit('closeAll')">
      </button>
    </div>

    <textarea class="existing_task__description"
      v-if="task.isAddjusted"
      v-bind:value="task.description"
      v-on:input="task.description = $event.target.value"
    ></textarea>
  </li>
</template>

<script>

  export default {
    props:['task'],

    data() {
      return {
         PRIORITIES: {
          1: "High",
          2: "Medium",
          3: "Low"
        },
        newValue: null
      }
    }
  }
</script>

<style scoped>

  .existing_task {
    display: flex;
    flex-direction: column;
    width: 330px;
  }

  .existing_task__wrapper {
    position: relative;
    display: flex;
    flex-direction: row;
    align-items: center;
  }

  .existing_task__name {
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
    border:none;
  }

  .existing_task__name--adjusted {
    border: 1px solid gray;
    background-color: white;
    margin-top: 12px;
    padding-top: 0;
    min-height: 40px;
  }


  .task__name--done {
    color: green;
    text-decoration: line-through;
  }

  .existing_task__description {
    font-size: 12px;
    line-height: 20px;
    padding-top: 10px;
    padding-bottom: 10px;
    padding-left: 10px;
    vertical-align: middle;
    width: 327px;
    min-height: 50px;
    border: grey solid 1px;
    border-radius: 5px;
    margin-top: 0;
  }

  .existing_task__priority {
    font-size: 12px;
    line-height: 20px;
    width: 60px;
    font-weight: 600;
  }

  .existing_task__priority--high {
    color: tomato;
  }

  .existing_task__priority--medium {
    color: orange;
  }

  .existing_task__priority--low {
    color: green;
  }

    .button--delete {
    height: 20px;
    width: 20px;
    position: absolute;
    bottom: -10px;
    right: 50px;
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
    right: 80px;
    padding: 0;
    border: none;
    background-image: url("./img/ok.svg");
    background-repeat: no-repeat;
    background-position-x: 5px;
    background-position-y: 5px;
    background-color: white;
    border-radius: 50%;
  }

  .button--done:disabled {
    background-color: rgba(0, 0, 0, 0.1);
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
    right: 25px;
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

  .button--shrink {
    background-image: url("./img/shrink.svg");
    background-position-x: 0;
    background-position-y: 2px;
    border: 2px solid indianred;
  }

</style>
