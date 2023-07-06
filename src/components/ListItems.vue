<template>
   <article class="item-list">
      <input type="text" v-model="newItem" @keypress.enter="addItem" placeholder="Add a new item" />
      <div class="list-father" v-if="list.length">
         <ul>
            <li v-for="item in list" :key="item.id" @click="itemDone(item)" :class="{ done: item.done }" class="item"> 
               {{item.description}} - {{item.id}}
               <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" @click="deleteItem(item.id)"></button>
            </li>
         </ul>
      </div>
      <div v-else>
         List Empty!
      </div>
   </article>
</template>

<script>
import { ref } from 'vue'

export default {
   setup(props, { emit }) {
      const list = ref([
         { id: 1, description: "Study for the exam!", done: false}
      ]);
      let counter = 2;
      const newItem = ref('');

      const addItem = function() {
         if (newItem.value) {
            const newId = counter;
            list.value.push({ id: newId, description: newItem.value });
            newItem.value = "";
            counter++;
         } else {
            emit("badValue");
         }
      }
      const itemDone = (item) => {
         item.done = !item.done;
      }

      const deleteItem = (id) => {
         list.value = list.value.filter(item => item.id !== id);
      }

      return {
         list,
         addItem,
         newItem,
         itemDone,
         deleteItem
      }
   },
}
</script>

<style scoped>
   .item-list {
      display: flex;
      flex-direction: column;
      align-items: center;
      row-gap: 5vh;
   }

   input {
      width: 20%;
      padding: 1vh;
   }

   .item-list ul {
      display: flex;
      flex-direction: column;
      row-gap: 4vh;
      width: 100%;
   }

   .item-list li {
      font-size: 24px;
      display: flex;
      width: 100%;
      height: 10vh;
      align-items: center;
      justify-content: space-between;
      padding-left: 5vh;
      border-radius: 25px;
      background-color: goldenrod;
      box-shadow: 1px 3px 5px rgba(0, 0, 0, .6);
   }

   .item-list li:hover {
      background-color: green;
      cursor: pointer;
   }

   .item-list .done {
      background-color: green;
      text-decoration: line-through;
   }
</style>
