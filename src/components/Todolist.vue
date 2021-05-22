<template>
  <div class="todolist">
    <center>
      <h1>Todo</h1>
      <div>
        <input
          type="text"
          placeholder="    What needs to be done"
          v-model="todoInput"
          @keyup.enter="addTodo"
          class="form-control"
        />
        &nbsp; &nbsp;
        <button
          type="button"
          class="btn btn-outline-success"
          id="addButton"
          @click="addTodo"
        >
          add
        </button>
      </div>
    </center>

    <br />
    <div v-for="(todo, index) in todoList" :key="index">
      <div class="uncomplete">
        <input
          id="checkbox"
          class="checkbox"
          type="checkbox"
          @click="todo.toggleChecked()"
          v-if="!todo.checked"
        />
        &nbsp; &nbsp;
        <div
          v-if="!todo.editing"
          @dblclick="editTodo(todo)"
          style="display:inline;"
        >
          <div v-if="!todo.checked" style="display:inline; ">
            <label>{{ todo.todo }}</label>
          </div>
        </div>

        <input
          v-else
          type="text"
          v-model="todo.todo"
          @blur="doneEdit(todo)"
          @keyup.enter="doneEdit(todo)"
        />
        <!-- <button @click="delTodo(todo)">delete</button> -->
      </div>
    </div>

    <div class="complete">
      <h2>Todo complete</h2>
      <div class="donetodo" v-for="(todo, index) in todoList" :key="index">
        <input
          id="checkbox"
          class="checkbox"
          type="checkbox"
          checked="true"
          @click="todo.toggleChecked()"
          v-if="todo.checked"
        />
        <div v-if="todo.checked" style="display:inline">
          <label class="donelist">{{ todo.todo }}</label>
        </div>
        &nbsp; &nbsp;
        <button class="delbutton" @click="delTodo(todo)" v-if="todo.checked">
          delete
        </button>
      </div>
    </div>
    <br />
    <br />
    <br />
    <br />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
// import { log } from "util";

class Todo {
  todo: string;
  checked: boolean;
  status: boolean;
  editing: boolean;
  constructor(todo: string) {
    this.todo = todo;
    this.checked = false;
    this.status = false;
    this.editing = false;
  }

  toggleChecked() {
    this.checked = !this.checked;

    if (this.checked == true) {
      this.status = true;
      // this.todo = "";
      console.log('complete');
    } else {
      this.status = false;
      console.log('uncomplete');
    }
  }
}

@Component
export default class Todolist extends Vue {
  //   @Prop() private msg!: string;
  todoInput: string = '';
  todoList: Todo[] = [];

  addTodo() {
    console.log('todoInput', this.todoInput);
    let todo = new Todo(this.todoInput);
    this.todoList.push(todo);

    // เมื่อเพิ่ม todo ช่อง input todo จะว่าง
    if (this.todoInput.length > 0) {
      this.$emit('on-new-todo', this.todoInput);
    }
    this.todoInput = '';
  }

  mounted() {
    this.todoList.push(new Todo('homework'));
    this.todoList.push(new Todo('reading'));
  }

  delTodo(todo: Todo) {
    console.log('delTodo:', todo);
    this.todoList = this.todoList.filter((todoItem) => {
      return todoItem !== todo;
    });
  }

  editTodo(todo: any) {
    todo.editing = true;
  }

  doneEdit(todo: any) {
    todo.editing = false;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@background: #4fc08d;
@white: rgb(255, 255, 255);
@gray: rgb(75, 75, 75);
div {
  background-color: @white;
  color: @gray;
}

// เมื่อติ๊ก checkbox จะขีดฆ่า todo ที่ทำเสร็จแล้ว
.donelist {
  text-decoration: line-through;
  color: grey;
}

// ช่องเพิ่ม todo
input[type='text'] {
  border-radius: 20px;
  width: 250px;
  height: 30px;
  display: inline;
  border-width: thin;
  border-color: #e1e1e1;
}

// ช่อง checkbox
input[type='checkbox'] {
  background-color: @background;
}

// ปุ่ม add todo
.btn-outline-success {
  color: @white;
  background-color: #2c3e50;
  border-radius: 20px;

  height: 30px;
  border-style: none;
  width: 70px;
}
.btn-outline-success:hover {
  color: #2c3e50;
  font-weight: bold;
  background-color: #e7e7e7;
}

// ปุ่ม delete
.delbutton {
  .btn-outline-success;
  background-color: #3aa374;
}
.delbutton:hover {
  color: @background;
  font-weight: bold;
  background-color: #e7e7e7;
}
.donetodo {
  text-align: left;
  padding: 5px;
  padding-left: 39%;
}
.uncomplete {
  .donetodo;
}
</style>
