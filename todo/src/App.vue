<template>
  <div id="app">
    <div class="container">
      <div class="row justify-content-md-center">
        <div class="col col-md-6">
          <div class="card">
            <div class="card-header">Add new task</div>
            <form class="card-body" @submit.prevent="add(todo)">
              <div class="input-group">
                <input type="text" class="form-control" placeholder="Add new task" v-model="todo" />
                <span class="input-group-btn">
                  <button class="btn btn-primary" type="submit" :disabled="!todoIsValid">Add task</button>
                </span>
              </div>
            </form>
            <ul class="list-group list-group-flush">
              <li class="list-group-item" v-for="todo in todos" :key="todo">
                <div class="d-flex" v-if="!todo.isEdited">
                  <div class="form-check m-2">
                    <input type="checkbox" class="form-check-input" v-model="todo.finished" />
                    <label class="form-check-label">
                      <span :class="{'todo-finished': todo.finished}">{{ todo.content }}</span>
                    </label>
                  </div>
                  <button
                    class="btn btn-sm btn-primary ml-auto"
                    @click="edit(todo)"
                    v-show="!isBeingEdited"
                  >
                    <i class="fa fa-edit"></i>
                  </button>
                </div>
                <form class="m-0" v-else @submit.prevent="update(todo)">
                  <div class="input-group">
                    <input
                      type="text"
                      class="form-control form-control-sm"
                      placeholder="Edit"
                      v-model="editTodo"
                    />
                    <div class="btn-group" role="group" aria-label="Basic example">
                      <button
                        class="btn btn-outline-primary"
                        type="submit"
                        :disabled="!editTodoIsValid"
                      >
                        <i class="fa fa-save"></i>
                      </button>
                      <button class="btn btn-outline-danger" @click="remove(todo)">
                        <i class="fa fa-trash"></i>
                      </button>
                    </div>
                  </div>
                </form>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data: () => ({
    todo: null,
    todos: [],
    editTodo: null
  }),
  computed: {
    todoIsValid() {
      return !!this.todo;
    },
    editTodoIsValid() {
      return !!this.editTodo;
    },
    isBeingEdited() {
      return this.todos.filter(todo => todo.isEdited).length > 0;
    }
  },
  methods: {
    add(todo) {
      if (!this.todoIsValid) return;
      this.todos.push({ content: todo, finished: false, isEdited: false });
      this.todo = null;
    },
    remove(todo) {
      this.todos = this.todos.filter(item => item !== todo);
    },
    edit(todo) {
      this.editTodo = todo.content;
      todo.isEdited = true;
    },
    update(todo) {
      if (!this.editTodoIsValid) return;
      todo.content = this.editTodo;
      todo.isEdited = false;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.todo-finished {
  text-decoration: line-through;
  color: rgb(128, 128, 128);
}
</style>
