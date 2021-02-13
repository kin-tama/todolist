<template>
  <li class="existing_tasks"
    v-on:keydown.esc="task.isAddjusted = false,
      task.isExpanded = false">

    <!-- <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="red" class="bi bi-x-circle" viewBox="0 0 16 16" style="display: none"> -->
    <svg style="display: none">

      <symbol viewBox="0 0 16 16" id="del">
        <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
        <path d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"/>
      </symbol>

      <symbol viewBox="0 0 16 16" id="done">
        <path d="M2.5 8a5.5 5.5 0 0 1 8.25-4.764.5.5 0 0 0 .5-.866A6.5 6.5 0 1 0 14.5 8a.5.5 0 0 0-1 0 5.5 5.5 0 1 1-11 0z"/>
        <path d="M15.354 3.354a.5.5 0 0 0-.708-.708L8 9.293 5.354 6.646a.5.5 0 1 0-.708.708l3 3a.5.5 0 0 0 .708 0l7-7z"/>
      </symbol>

      <symbol viewBox="0 0 16 16" id="expand">
        <path fill-rule="evenodd" d="M3.646 10.146a.5.5 0 0 1 .708 0L8 13.793l3.646-3.647a.5.5 0 0 1 .708.708l-4 4a.5.5 0 0 1-.708 0l-4-4a.5.5 0 0 1 0-.708zm0-4.292a.5.5 0 0 0 .708 0L8 2.207l3.646 3.647a.5.5 0 0 0 .708-.708l-4-4a.5.5 0 0 0-.708 0l-4 4a.5.5 0 0 0 0 .708zM1 8a.5.5 0 0 1 .5-.5h13a.5.5 0 0 1 0 1h-13A.5.5 0 0 1 1 8z"/>
      </symbol>

      <symbol viewBox="0 0 16 16" id="concat">
        <path fill-rule="evenodd" d="M3.646 14.854a.5.5 0 0 0 .708 0L8 11.207l3.646 3.647a.5.5 0 0 0 .708-.708l-4-4a.5.5 0 0 0-.708 0l-4 4a.5.5 0 0 0 0 .708zm0-13.708a.5.5 0 0 1 .708 0L8 4.793l3.646-3.647a.5.5 0 0 1 .708.708l-4 4a.5.5 0 0 1-.708 0l-4-4a.5.5 0 0 1 0-.708zM1 8a.5.5 0 0 1 .5-.5h13a.5.5 0 0 1 0 1h-13A.5.5 0 0 1 1 8z"/>
      </symbol>

    </svg>

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
        <svg width="16" height="16">
          <use xlink:href="#done"></use>
        </svg>
      </button>

      <button class="button button--delete" :id="task.id" v-on:click="$emit('deleteItem', task.id)">
        <svg width="16" height="16">
          <use xlink:href="#del"></use>
        </svg>
      </button>

      <button class="button button--expand"
        v-on:click="task.isAddjusted = !task.isAddjusted; $emit('closeAll')">
        <svg width="14" height="14">
          <use v-if="!task.isAddjusted" xlink:href="#expand"></use>
          <use v-if="task.isAddjusted" xlink:href="#concat"></use>
        </svg>
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
    props:{
      task: {
        type: Object,
        required: true
      }
    },

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
    font-weight: 400;
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
    padding-top: 2px;
    width: 20px;
    border-radius: 50%;
    border: none;
    background-color: white;
    fill: hotpink;
  }

  .button--delete:hover {
    background-color: rgba(0, 0, 0, 0.1);
    fill: red;
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
    padding-top: 2px;
    border: none;
    background-color: white;
    border-radius: 50%;
    fill:lightseagreen
  }

  .button--done:disabled {
    background-color: rgba(0, 0, 0, 0.1);
  }

  .button--done:hover {
    background-color: rgba(0, 0, 0, 0.1);
    fill: green

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
    right: 20px;
    padding: 0;
    border: none;
    background-color: ghostwhite;
    border-radius: 2px;
    fill: gray;
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

</style>
