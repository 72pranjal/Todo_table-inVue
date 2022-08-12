<template>
  <main class="app">
    <h2>Todo List</h2>
    <section>
      <h2>
        What's up,
       
      </h2>
    </section>
    <section>
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input
          type="text"
          placeholder="e.g make a video"
          v-model="input_content"
          class="aaaaa"
        />

        <h4>Pick a Category</h4>

        <div>
          <label>
            <input
              type="radio"
              name="category"
              value="business"
              v-model="input_category"
            />
            <span></span>
            <div>Business</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="personal"
              v-model="input_category"
            />
            <span></span>
            <div>Personal</div>
          </label>
        </div>
        <h4 class="error">{{ msg }}</h4>
        <input type="submit" class="btn" value="Add Todo" />
      </form>
    </section>
    <section>
      <h3>TODO LIST</h3>
      <div class="table-list">
        <table>
          <thead>
            <tr>
              <th>Id</th>
              <th>Name</th>
              <th>Category</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(todo, index) in todos_asc" :key="index">
            <td>{{index+1}}</td>
              <td>{{ todo.content }}</td>
              <td>{{ todo.category }}</td>
              <td><button class="btn" @click="removeTodo(todo)">Delete</button></td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
    <!-- {{todos_asc}} -->
  </main>
</template>

<script  setup>
const { ref } = require("@vue/reactivity");
const { computed, watch, onMounted } = require("@vue/runtime-core");

const todos = ref([]);
const name = ref("");
const input_content= ref("");
const input_category = ref(null);
const msg = ref("");

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return a.content.localeCompare(b.content)
  })
);

const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value == null) {
    return (msg.value = "Please enter todo list name and also choose category..?");
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    // createdAt: new Date().getTime(),
  });
  input_content.value = "";
  input_category.value = null;
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.app {
  /* border: 2px solid black; */
  background-color: lightpink;
  width: auto;

  margin-left: auto;
  margin-right: auto;
}
body {
  background-color: lightgreen;
}
.table-list{
  padding-left:700px;
 
  
}
th{
  text-align: center;
  border: 1px solid black;
  background-color: greenyellow;
  color: red;
  height: 35px;
  width: 100px;
}
td{
  border: 1px solid black;
height: 30px;
  width: 100px;
  color:blue;
}
.btn {
  background-color:#4CAF50;
  height: 25px;
  color: white;
   display: inline-block;
   font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  text-decoration: none;
  border: none;
  font-weight: 400;
    --tw-bg-opacity: 1;
    background-color: rgb(66 133 244 / var(--tw-bg-opacity));
--tw-shadow: 0 0 3px 3px #4285f4;
    --tw-shadow-colored: 0 0 3px 3px var(--tw-shadow-color);
    box-shadow: var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow);
  
}
.error{
 color: rgb(0, 3, 90);
}
.aaaaa{
  display: block;
  width: 300px;
  padding: 10px;
  margin-left: auto;
  margin-right: auto;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  
}


</style>
