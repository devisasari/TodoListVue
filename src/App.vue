<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {
  deep: true
})

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime()
  })
}

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo)
}

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
  <main class="app">

    <section class="create-todo">
      <center>
        <h2>YAPILACAKLAR</h2>
      </center>

      <form id="new-todo-form" @submit.prevent="addTodo">
        <h4>Listenizi oluşturun</h4>
        <input type="text" name="content" id="content" placeholder="örnek: mailleri gönder" v-model="input_content" />

        <h4>Bir kategori seçin</h4>
        <div class="options">

          <label>
            <input type="radio" name="category" id="category1" value="projects" v-model="input_category" />
            <span class="bubble projects"></span>
            <div>Projeler</div>
          </label>

          <label>
            <input type="radio" name="category" id="category2" value="school" v-model="input_category" />
            <span class="bubble school"></span>
            <div>Okul</div>
          </label>

          <label>
            <input type="radio" name="category" id="category3" value="home" v-model="input_category" />
            <span class="bubble home"></span>
            <div>Ev</div>
          </label>

          <label>
            <input type="radio" name="category" id="category4" value="shopping" v-model="input_category" />
            <span class="bubble shopping"></span>
            <div>Alışveriş</div>
          </label>

        </div>

        <input type="submit" value="Ekle" />
      </form>
    </section>
    <section class="todo-list">
      <center>
        <h2>LİSTEM</h2>
      </center>
      <div class="list" id="todo-list">

        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category == 'projects'
            ? 'projects'
            : 'school'
            }`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Sil</button>
          </div>
        </div>

      </div>
    </section>

  </main>
</template>