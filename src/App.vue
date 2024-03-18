<template>
  <div class="app">
    <main>
      <AddToDo :fetchTasks="fetchTasks" @new-item="insertNew" />
      <TodoList :list="list" :removeItem="removeItem"  :editItem="editItem"/>
    </main>
  </div>
</template>

<script setup>
import { collection, deleteDoc, doc, getDocs, updateDoc } from 'firebase/firestore';
import { onMounted, ref } from 'vue';
import AddToDo from './components/AddToDo.vue';
import TodoList from './components/TodoList.vue';
import db from './firebase.js';

const list = ref([]);


const fetchTasks = async () => {
  try {
    // Fetch tasks from the Firestore database
    const querySnapshot = await getDocs(collection(db, 'tasks'));
    list.value = querySnapshot.docs.map((doc) => ({ id: doc.id, ...doc.data() }));
  } catch (error) {
    console.error('Error fetching tasks:', error.message);
  }
};

const removeItem = async (taskId) => {
  try {
    // Remove task from the Firestore database
    await deleteDoc(doc(db, 'tasks', taskId));
    // Fetch updated task list
    await fetchTasks();
  } catch (error) {
    console.error('Error removing task:', error.message);
  }
};

const editItem = async (task) => {

  console.log("edited task is", task)
  const updatedTaskTitle = prompt('Edit task:', task.taskTitle);
  if (updatedTaskTitle !== null) {
    try {
      // Update task in the Firestore database
      await updateDoc(doc(db, 'tasks', task.id), { taskTitle: updatedTaskTitle });
      // Fetch updated task list
      await fetchTasks();
    } catch (error) {
      console.error('Error editing task:', error.message);
    }
  }
};
onMounted(()=>{fetchTasks()})
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    outline: 0;
    border: none;
    font-family: 'Inter', sans-serif;
  }

  @media (max-width: 1080px) {
    html {
      font-size: 93.75%;
    }
  }

  @media (max-width: 720px) {
    html {
      font-size: 87.5%;
    }
  }

  body {
    background-color: #CEDFF3;
    color: #fff;
  }

  .app {
    height: 96vh;
    width: 95vw;
    margin: 2vh auto;
    padding: 1.3rem;
    background: #CEDFF3;;
    border-radius: .75rem;
    display: flex;
    justify-content: center;
  }

  main {
    background: #CEDFF3;;
    border-radius: .75rem;
    padding: 2rem 1rem;
    height: 100%;
    box-shadow: 0 0 1rem .075rem #0006;
    display: flex;
    flex-direction: column;
    gap: 2rem;
    width: 50%;
  }

</style>
