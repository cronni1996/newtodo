<template>
  <div class="container">
    <div id="app1">
      <h1>ToDo List</h1>
      <input v-model="itemName" @keyup.enter="addItem" type="text"/><br/>
      <button @click="addItem()">Add Item</button>
    </div>
    <ol>
      <li class="todo-li"

          v-for="item of items"
          :class="{ bought: item.bought }"
          :key="item.id"

      >
        <input class="checkbox" id="checkbox" type="checkbox" required @click="boughtItem(item.id )">
        <label for="checkbox"></label>{{ item.name }}

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

      var chbox;
      chbox = document.getElementById('checkbox');
      if (chbox.checked) {
        await axios.patch(`http://localhost:3000/items/${id}`, {
          bought: true,
        });
      } else {
        await axios.patch(`http://localhost:3000/items/${id}`, {
          bought: false,
        });
      }
      this.items = this.items.map((item) => {
        if (item.id === id) {
          item.bought = !item.bought;
        }
        return item;
      });
    },

    completed(id, checked) {
      if (checked === true) {
        axios.patch(`http://localhost:3000/items/${id}`, {
          bought: true,
        });
      } else {
        if (checked === true) {
          axios.patch(`http://localhost:3000/items/${id}`, {
            bought: false,
          });
        }
      }
      this.items = this.items.map((item) => {
        if (item.id === id) {
          item.bought = true;
        }
        return item;
      });
    },

    removeItem(id) {
      axios.delete(`http://localhost:3000/items/${id}`);
      this.items = this.items.filter((item) => item.id !== id);
    },

    async addItem() {

      if (this.itemName === "") {
        alert('Заполните все поля')
      } else {
        const res = await axios.post(`http://localhost:3000/items`, {
          name: this.itemName,
          bought: false
        });
        this.items = [...this.items, res.data];
        this.itemName = "";
      }
    }
  },
};
</script>

<style>
html {
  background-image: url("https://getwallpapers.com/wallpaper/full/0/3/0/135390.jpg");
}

#app {
  width: 80%;
  margin-top: 3%;
  margin-left: 12%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
  border: solid 2px black;
  background: rgba(204, 234, 200, 0.28);
}

h1 {
  font-family: cursive;
  font-size: 52px;
}


.checkbox {
  left: 2px;
  position: relative;
  float: left;
  z-index: 20;
  opacity: 0;
  margin: 2px -50px 0 2px;
  width: 49px;
  height: 33px;
}

.checkbox + label {
  float: left;
  position: relative;
  padding: 5px 0 0 60px;
  cursor: pointer;
}

.checkbox + label:before {
  content: '';
  position: absolute;
  top: 3px;
  left: 2px;
  width: 50px;
  height: 26px;
  border-radius: 13px;
  background: #CDD1DA;
  box-shadow: inset 0 2px 3px rgba(0, 0, 0, .2);
  transition: .2s;
}

.checkbox + label:after {
  content: '';
  position: absolute;
  top: 5px;
  left: 4px;
  width: 22px;
  height: 22px;
  border-radius: 10px;
  background: #FFF;
  box-shadow: 0 2px 5px rgba(0, 0, 0, .3);
  transition: .2s;
}

.checkbox:checked + label:before {
  background: #9FD468;
}

.checkbox:checked + label:after {
  left: 26px;
}

.checkbox:focus + label:before {
  box-shadow: inset 0 2px 3px rgba(0, 0, 0, .2), 0 0 0 3px rgba(255, 255, 0, .7);
}


.container {
  margin: 0 auto;
  border-radius: 0;
}

li {
  height: 32px;
  list-style-type: none;
  font-size: 1.5rem;
  border-radius: 10px;
  border: solid 3px #3498db;
  background: white;
  float: top;
  margin: 5px 28px 0 28px;
}

ol {
  padding-left: 0;
}

.button-completed {
  background: red;
  color: white;
  border-radius: 50%;
  font-weight: bold;
  margin-top: 0;
  float: right;
  text-align: center;

  padding: 2px 8px;
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
  font-size: 20px;
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