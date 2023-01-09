<template>
  <div class="container">
    <div id="app">
      <h1>ToDo List</h1>
      <input v-model="itemName" @keyup.enter="addItem" type="text" /><br />
      <button @click="addItem()">Add Item</button>
    </div>
    <ol>
      <li class="todo-li"

          v-for="item of items"
          :class="{ bought: item.bought }"
          :key="item.id"

          @click="boughtItem(item.id)"
      >
        <input class="checkbox" type="checkbox"   @click="boughtItem(item.id)">
        {{ item.name }}
        <button class="button-completed" v-on:click="removeItem(item.id)">X</button>
      </li>
    </ol>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "App",

  data() {
    return {
      items: [],
      itemName: "",
    };
  },
  async created() {
    try {
      const res = await axios.get(`http://localhost:3000/items`);
      this.items = res.data;
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    async boughtItem(id) {
      await axios.patch(`http://localhost:3000/items/${id}`, {
        bought: true,
      });
      this.items = this.items.map((item) => {
        if (item.id === id) {
          item.bought = true;
        }
        return item;
      });
    },
    //on double clicking the item, it will call removeItem(id) method
    removeItem(id) {
      axios.delete(`http://localhost:3000/items/${id}`);
      this.items = this.items.filter((item) => item.id !== id);
    },
    //method for adding items in the list
    async addItem() {
      const res = await axios.post(`http://localhost:3000/items`, {
        name: this.itemName,
      });
      this.items = [...this.items, res.data];
      this.itemName = "";
    },
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
}
.checkbox{
  float: left;
  margin-top: 10px;
  border-radius: 30px;
}
.container {

  margin: 0 auto;
  border-radius: 8px;
}
li {
  font-size: 1.5rem;
  border-radius: 10px;
  border: solid 3px #3498db;
  background: white;
  float: top;
  margin: 5px 0px 0px 0px;

}
.button-completed{
  background: red;
  color: white;
  border-radius: 50%;
  font-weight: bold;

  float: right;
  text-align: center;

  padding: 1px 6px;
  border-width: 2px;
  border-style: outset;
  border-image: initial;
}

button {
  margin-top: 5px;
  background-color: #3498db;
  border: none;
  color: #ffffff;
  padding: 10px 20px;
  font-size: 14px;
  cursor: pointer;
  border-radius: 4px;
}
input {
  margin-top: 5px;
  padding: 10px 20px;
  font-size: 14px;
  border-radius: 4px;
}
.bought {
  text-decoration: line-through;
}
</style>