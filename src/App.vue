<template>
  <div
    class="container border border-3 border-dark p-4"
    style="max-width: 500px"
  >
    <h1><span class="text-success">Vue 3</span> Todo App</h1>
    <form @submit.prevent="addItem" class="my-4">
      <div class="input-group">
        <input
          name="text"
          v-model="text"
          class="form-control"
          placeholder="Enter the value..."
        />
        <button class="btn btn-success">Add Item</button>
      </div>
    </form>
    <div v-if="list.length > 0">
      <ul class="list-group">
        <li
          class="
            list-group-item
            my-1
            border border-3
            d-flex
            justify-content-between
            align-items-center
          "
          :key="item.id"
          v-for="(item, index) in list"
          @dblclick="toggleDone(item)"
          :class="{ 'border-success': item.done }"
        >
          <h3 :class="{ 'text-success': item.done }">
            {{ item.title }}
            <span v-if="item.done" class="text-danger mx-1"
              ><i class="fas fa-check"></i
            ></span>
          </h3>
          <div>
            <i
              class="fas fa-edit text-success mx-2"
              @click="editItem(item.id)"
            ></i>
            <i class="fas fa-times text-danger" @click="removeItem(index)"></i>
          </div>
        </li>
      </ul>
      <button
        id="clear-btn"
        class="btn btn-danger w-100 my-3"
        @click="clearList"
      >
        CLEAR LIST
      </button>
    </div>
    <div v-else><h2>LIST IS EMPTY!</h2></div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const text = ref('');
    const list = ref([]);
    let editing = ref(false);
    let editID = ref(null);

    function addItem() {
      if (!text.value) {
        alert('please enter the value!');
        return;
      } else if (text.value && editing) {
        const newList = list.value.map((item) => {
          if (item.id === editID) {
            return { ...item, title: text.value };
          } else {
            return item;
          }
        });
        list.value = newList;
      }
      list.value.push({
        id: new Date().getTime().toString(),
        done: false,
        title: text.value,
      });
      text.value = '';
      editing = false;
      editID.value = null;
    }

    function toggleDone(item) {
      item.done = !item.done;
    }

    function removeItem(index) {
      list.value.splice(index, 1);
    }

    function clearList() {
      list.value = [];
    }

    function editItem(id) {
      const specificItem = list.value.find((item) => item.id === id);
      text.value = specificItem.title;
      editing = true;
      editID.value = id;
    }

    return {
      text,
      addItem,
      list,
      toggleDone,
      removeItem,
      clearList,
      editItem,
    };
  },
};
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
#clear-btn {
  letter-spacing: 3px;
}
h2 {
  letter-spacing: 3px;
}
h3 {
  text-transform: capitalize;
}
</style>
