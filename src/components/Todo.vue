<template>
  <div class="container">
    <h3>{{ header }} ({{ count }})</h3>
    <div class="row justify-content-center">
      <div class="col-md-6">
        <form @submit.prevent="addTask" class="row">
          <input
            type="text"
            v-model="title"
            class="form-control col-md-9 mr-1"
            placeholder="Write the task"
          />
          <button class="btn btn-primary col-md-2">Add New</button>
        </form>
        <table class="table table-striped table-bordered mt-5">
          <tr class="mb-3" v-for="(todo, index) in todos" :key="index">
            <td>
                <input
                  type="checkbox" v-model="todo.completed"
                  @click="check(index)"
                  class="form-control-checkbox"
                  checked
                />
            </td>
            <td>
              <span :class="{ done: todo.completed }">{{ todo.title }}</span>
            </td>
            <td>
              <button
                v-if="todo.completed"
                @click="deleteTask(index)"
                class="btn btn-danger"
              >
                Delete
              </button>
            </td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Todo",
  data() {
    return {
      header: "To Do list (Vue)",
      title: "",
      todos: [],
    };
  },
  methods: {
    view() {
      if(localStorage.getItem('tasks'))
        this.todos = JSON.parse(localStorage.getItem('tasks'));
    },
    addTask() {
      if (this.title.length != 0) {
        let task = {
          title: this.title,
          completed: false,
        };
        this.todos = [...this.todos, task];
        localStorage.setItem('tasks', JSON.stringify(this.todos));
        this.title = "";
      }
    },
    check(index) {
      if(this.todos[index].completed === false) {
        this.todos[index].completed = true;
      } else {
        this.todos[index].completed = false;
      }
      localStorage.setItem('tasks', JSON.stringify(this.todos))
      this.view()
    },
    deleteTask(index) {
      this.todos.splice(index, 1);
      localStorage.setItem('tasks', JSON.stringify(this.todos))
      this.view()
    },
  },
  computed: {
    count() {
      let total = this.todos.filter((todo) => todo.completed === false);
      return total.length;
    },
  },
  created() {
    this.view();
  }
};
</script>


<style >
.done {
  text-decoration: line-through;
}
</style>

