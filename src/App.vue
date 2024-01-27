<template>
  <main>
    <section>
      <h2>
        What's up,
        <input type="text" placeholder="Name here..." v-model="name" />
        {{ name }}
      </h2>
    </section>

    <section>
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input
          v-model="input_comtent"
          type="text"
          placeholder="e.g make video.."
        />
        <h4>Pick a category</h4>

        <div class="flex justify-center space-x-5">
          <div class="bg-red-300 max-w-[100px]">
            <input 
              type="radio" 
              name="cat egory"
              value="business" 
              v-model="input_category" />

              <span></span>
              <div>Business</div>
          </div>

          <div class="bg-red-300 max-w-[100px]">
            <input 
              type="radio" 
              name="cat egory"
              value="personal" 
              v-model="input_category" />

              <span></span>
              <div>Personal</div>
          </div>
        </div>

        <input type="submit" value="addTodo" class="border justify-center flex w-full" />
      </form>
    </section>

    <section>
      <h3>TODO LIST</h3>
      <div class="w-full space-y-3 mt-5">
        <div v-for="(todo, index) in todos_asc" 
        :key="index"
        :class="`line-through ${todo.done && 'done'}`"
        class="flex space-x-3 border-2"
        >

        <label>
          <input type="checkbox" v-model="todo.done">
          <span :class="`bubble ${todo.category}`"></span>
        </label>

        <div>
          <input type="text" v-model="todo.content">
        </div>

        <div>
          <button @click="removeTodo(todo)">Delete</button>
        </div>

        </div>
      </div>
    </section>

  </main>
</template>

<script setup>
import { ref, computed, onMounted, watch } from "vue";

const todos = ref([]);

const name = ref("");

const input_comtent = ref("");
const input_category = ref(null);

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const addTodo = () => {
  if (input_comtent.value.trim() === "" || input_category.value === null) {
    return ;
  }
  todos.value.push({
    content: input_comtent.value,
    category: input_category.value,
    done: false,
    date: new Date().getTime()
  })
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter(t => t != todo)
}

watch(todos, newVal => {
  localStorage.setItem("todos", JSON.stringify(newVal))
}, {deep: true})

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>
