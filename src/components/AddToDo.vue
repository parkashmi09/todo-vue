<template>
  <div class="input-container">
    <input v-model="title" type="text" placeholder="Task" @keydown.enter="insertNewItem">
    <button @click="insertNewItem">+</button>
  </div>
</template>

<script setup>
import { addDoc, collection } from 'firebase/firestore';
import { defineProps, ref } from 'vue';
import db from '../firebase.js';

const title=ref('');
const props = defineProps(['fetchTasks']);

const insertNewItem = async () => {
  try {
    if(title.value===''){
 window.alert('Task title cannot be empty!');
    }
    else{
      const taskTitle = title.value;
      // Add task to the Firestore database
     await addDoc(collection(db, 'tasks'), { taskTitle});
     title.value=''
      // Fetch updated task list
      props.fetchTasks();
    }
  
  } catch (error) {
    console.error('Error inserting new task:', error);
  }
};

</script>

<style scoped>
  .input-container {
    height: 3rem;
    display: flex;
  }

  .input-container input {
    text-indent: 1rem;
    flex: 1;
    font-size: 1.2rem;
    height: 3rem;
    border-top-left-radius: .5rem;
    border-bottom-left-radius: .5rem;
  }

  .input-container button {
    width: 3rem;
    height: 3rem;
    border-top-right-radius: .5rem;
    border-bottom-right-radius: .5rem;
    cursor: pointer;
    background-color: #3DA5D9;
    color: #fff;
    font-size: 1.5rem;
  }
</style>