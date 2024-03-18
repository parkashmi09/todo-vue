<template>
  <div class="list-container">
    <ul>
      <li v-for="item in props.list" :key="item.title">
        <div class="item">
          <div :class="['item-title', { done: item.done }]" @click="markItemAsDone(item)">
            {{ item.taskTitle }} 
          </div>
           <div class="edit-item" @click="edit(item)">
            <button><i class="fa-regular fa-pen-to-square"></i></button>
          </div>
          <div class="remove-item" @click="remove(item.id)">
            <button>X</button>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

const props= defineProps({
  list: {
    type: Array,
   default:()=>[]
  },
   removeItem: {
    type: Function,
    required: true,
  },
   editItem: {
    type: Function,
    required: true,
  },
})

const remove = (taskId) => {
  // Call the removeItem method from props
  props.removeItem(taskId);
};
const edit = (task) => {
  // Call the removeItem method from props
  props.editItem(task);
};

</script>

<style scoped>
  .list-container {
    flex: 1;
    border-radius: .75rem;
    padding: 1rem 1rem;
    border: 2px solid #427AA1;
    overflow-y: auto;
  }

  ul {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  li {
    padding: 10px;
    border: .8px solid #3336;
    border-radius: .75rem;
    background-color: #F49D6E;
    color:  #000;
  }

  li div.item {
    display: flex;
    justify-items: flex-end;
    justify-content: flex-end;
    gap: 1rem;
  }

  li div.item-title {
    align-self: center;
    flex: 1;
    position: relative;
    text-decoration: line-through;
    text-decoration-color: transparent;
    text-decoration-thickness: .2em;
    transition-property: text-decoration;
    transition-duration: .2s;
  }




  li div.item .remove-item {
    align-self: center;
    height: 2rem;
    width: 2rem;
  }

  li div.item .remove-item button {
    background-color: #DDDFDF;
    color: #000;
    height: 100%;
    width: 100%;
    border-radius: .3rem;
    cursor: pointer;
    transition: background-color .2s;
  }
  li div.item .edit-item button {
    background-color:#3DA5D9;
    color: #000;
    height: 100%;
    width: 100%;
    border-radius: .3rem;
    cursor: pointer;
    transition: background-color .2s;
    padding: 4px 10px;
  }
    li div.item .edit-item button:hover {
      opacity: 0.8;
    }

  li div.item .remove-item button:hover {
    background-color: #c13;
    color: white;
  }
</style>