<script>
export default {
  data() {
    return {
      name: 'John Doe',
      status: 'active',
      tasks: [
        'Task 1',
        'Task 2',
        'Task 3',
      ],
      link: 'https://google.com',
    };
  },
  methods: {
    toggleStatus() {
      if (this.status === 'active') {
        this.status = 'pending';
      } else if (this.status === 'pending') {
        this.status = 'inactive';
      } else {
        this.status = 'active';
      }
    }
  },
};
</script>


<template>
  <div>
    <h1>{{ name }}</h1>
    <p v-if="status === 'active'">User is active</p>
    <p v-else-if="status === 'pending'">User is pending</p>
    <p v-else>User is inactive</p>

    <h3>Tasks</h3>
    <ul>
      <li v-for="task in tasks" :key="task">{{ task }}</li>
    </ul>
    <!-- <a v-bind:href="link">Click for google</a> -->
    <a :href="link">Click for google</a>
    <!-- <button v-on:click="toggleStatus">Toggle Status</button> -->
    <button @click="toggleStatus">Toggle Status</button>
  </div>
</template>

<!-- Composite API code from App.vue before starting vue.jobs -->
<script setup>
import { ref } from 'vue';
import { onMounted } from 'vue';

  const name = ref('John Doe');
  const status = ref('active');
  const tasks = ref([
    'Task 1',
    'Task 2',
    'Task 3',
  ]);
  const newTask = ref('');

  const toggleStatus = () => {
    if (status.value === 'active') {
      status.value = 'pending';
    } else if (status.value === 'pending') {
      status.value = 'inactive';
    } else {
      status.value = 'active';
    }
  }

  const addTask = () => {
    if (newTask.value.trim() !== '') {
      tasks.value.push(newTask.value);
      newTask.value = '';
    }
  }

  const deleteTask = (index) => {
    tasks.value.splice(index, 1);
  }

  onMounted(async () => {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await response.json();
      tasks.value = data.map((task) => task.title);
    } catch (error) {
      console.log('Error fetching tasks');
    }
  });
</script>


<template>
  <div>
    <h1>{{ name }}</h1>
    <p v-if="status === 'active'">User is active</p>
    <p v-else-if="status === 'pending'">User is pending</p>
    <p v-else>User is inactive</p>

    <form @submit.prevent="addTask">
      <label for="newTask">Add task</label>
      <input type="text" id="newTask" name="newTask" v-model="newTask">
      <button type="submit">Submit</button>
    </form>

    <h3>Tasks</h3>
    <ul>
      <li v-for="(task, index) in tasks" :key="task">
        <span>{{ task }}</span>
        <button @click="deleteTask(index)">x</button>
      </li>
    </ul>
    <br>
    <!-- <button v-on:click="toggleStatus">Toggle Status</button> -->
    <button @click="toggleStatus">Toggle Status</button>
  </div>
</template>
