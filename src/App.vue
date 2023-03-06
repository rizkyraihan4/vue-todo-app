<template>
  <div class="container" style="margin-top: 20px;">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO APP</h5>
        <div class="row">
          <div class="col-10">
            <input v-model="todo" @keyup.enter="addTodo" type="text" class="form-control">
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="addTodo">ADD</button>
          </div>
        </div>
        <list :todos="list" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
        <br>
        <small>Total Todo : {{ totalTodo }}</small>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, toRefs, onMounted, computed } from "vue";
import List from './components/ListTodo.vue'

export default {
  components: {
    List
  },
  setup() {
    const todo = ref('');
    const todos = reactive({
      list: []
    })

    onMounted(() => {
      const items = localStorage.getItem('todos')
      todos.list = items ? JSON.parse(items) : [];

    })

    const totalTodo = computed(() => {
      return todos.list.length
    })

    const addTodo = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false
      })
      todo.value = "";
      saveToLocalStorage();
    }

    const deleteTodo = todoIndex => {
      todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item
        }
      }),
        saveToLocalStorage();
    }

    const doneTodo = todoIndex => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = !item.isDone
        }
        return item;
      }),
        saveToLocalStorage();
    }

    // const finishedTodo = index => {
    //   todos[index].isDone = !todos[index].isDone
    //   saveToLocalStorage()
    // }

    const saveToLocalStorage = () => {
      localStorage.setItem('todos', JSON.stringify(todos.list))
    }

    return {
      todo,
      ...toRefs(todos),
      totalTodo,
      addTodo,
      deleteTodo,
      doneTodo,
      // finishedTodo
    }
  },

}
</script>


