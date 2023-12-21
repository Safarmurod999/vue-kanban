<script lang="ts">
import { ref } from "vue";
export default {
  name: "App",
  setup() {
    interface item {
      id: number;
      title: string;
      categoryId: number;
      date: any;
      phone: string;
    }
    const date = new Date().toLocaleDateString();

    const title = ref("");
    const phone = ref("");

    const items = ref([
      {
        id: 0,
        title: "Audi",
        categoryId: 0,
        date,
        phone: "+9989123456",
      },
      {
        id: 1,
        title: "Volvo",
        categoryId: 0,
        date,
        phone: "+9989123456",
      },
      {
        id: 2,
        title: "Mersedes",
        categoryId: 0,
        date,
        phone: "+9989123456",
      },
    ]);
    const categories = ref([
      {
        id: 0,
        title: "Yangi",
        colors: ["#757575", "#E6E6E6"],
      },
      {
        id: 1,
        title: "Amalda",
        colors: ["#FFCD29", "#FFE8A3"],
      },
      {
        id: 2,
        title: "Kutiluvchi",
        colors: ["#FFA629", "#FFA629"],
      },
      {
        id: 3,
        title: "Sifatsiz",
        colors: ["#F24822", "#FFC7C2"],
      },
      {
        id: 4,
        title: "Sifatli",
        colors: ["#14AE5C", "#AFF4C6"],
      },
    ]);

    function onDragstart(e: DragEvent, item: item): any {
      console.log(e.dataTransfer);
      e.dataTransfer.dropEffect = "move";
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.setData("itemId", item.id.toString());
    }
    function onDrop(e: DragEvent, categoryId: number): any {
      const itemId = parseInt(e.dataTransfer.getData("itemId"));
      items.value = items.value.map((x) => {
        if (x.id == itemId && x.categoryId < categoryId) {
          x.categoryId = categoryId;
        }
        return x;
      });
    }
    function addTodo(): any {
      items.value.push({
      id: items.value.length + 1,
      title: title.value,
      categoryId: 0,
      date,
      phone: phone.value,
    });
    }
    return {
      items,
      title,
      phone,
      categories,
      onDragstart,
      onDrop,
      addTodo,
    };
  },
};
</script>

<template>
  <form @submit.prevent="addTodo" class="todo-add">
    <label for="title">Vazifa nomi :</label>
    <input type="text" id="title" v-model="title" />
    <label for="phone">Telefon raqam :</label>
    <input type="text" id="phone" v-model="phone" />
    <button type="submit">+ Yangi vazifa qo'shish</button>
  </form>
  <div class="droppable-container">
    <div
      class="droppable"
      v-for="category in categories"
      :key="category.id"
      @drop="onDrop($event, category.id)"
      @dragover.prevent
      @dragenter.prevent
    >
      <h2 :style="`background-color:${category.colors[0]}`">
        {{ category.title }}
      </h2>
      <div
        class="draggable-container"
        :style="`background-color:${category.colors[1]}`"
      >
        <div
          v-for="item in items.filter(
            (item) => item.categoryId === category.id
          )"
          :key="item.id"
          @dragstart="onDragstart($event, item)"
          class="draggable"
          draggable="true"
        >
          <div class="draggable-top">
            <h5>{{ item.title }}</h5>
            <p>{{ item.date }}</p>
          </div>
          <h6>{{ item.phone }}</h6>
        </div>
      </div>
    </div>
  </div>
</template>
<style>
* {
  text-align: center;
  font-family: sans-serif;
}
#app {
  display: block;
  width: 100%;
  max-width: 1700px;
  margin-inline: auto;
}
.todo-add {
  display: flex;
  align-items: center;
  gap: 20px;
  @media (max-width:900px) {
    flex-direction: column;
  }
}
.todo-add button {
  padding: 10px 20px;
  background: #14ae5c;
  color: #fff;
  border-radius: 3px;
  border: 3px solid #11934e;
}
.todo-add input {
  border: 2px solid #14ae5c;
  border-radius: 3px;
  padding: 10px;
  text-align: left;
  outline: none;
}
.droppable-container {
  display: grid;
  width: 100%;
  grid-template-columns: repeat(5, 1fr);
  margin-inline: auto;
  grid-gap: 20px;
  @media (max-width:1300px) {
    grid-template-columns: repeat(4,1fr);
  }
  @media (max-width:1100px) {
    grid-template-columns: repeat(3,1fr);
  }
  @media (max-width:1000px) {
    grid-template-columns: repeat(3,1fr);
  }
  @media (max-width:800px) {
    grid-template-columns: repeat(2,1fr);
  }
  @media (max-width:550px) {
    grid-template-columns: 1fr;
  }
}
.droppable {
  width: 100%;
  border-radius: 5px;
  margin-bottom: 10px;
}
.droppable h2 {
  padding: 10px 20px;
  margin-bottom: 15px;
  font-size: 15px;
  border-radius: 3px;
  color: #fff;
}
.draggable-container {
  padding: 10px;
  border-radius: 3px;
  min-height: 243px;
}
.draggable {
  background: #fff;
  padding: 10px;
  border-radius: 5px;
  margin-bottom: 10px;
  border: 3px solid #bfbfbf;
}
.draggable-top {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}
h5 {
  color: #545454;
  text-decoration: underline;
  margin: 0;
}
p {
  margin: 0;
  font-size: 12px;
  color: #bfbfbf;
}
h6 {
  text-align: left;
  margin: 0;
  font-size: 10px;
  color: #bfbfbf;
}
</style>
