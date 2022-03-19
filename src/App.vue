<template>
  <div class="container">
    <input
      class="col-12"
      placeholder="Siteye Ekle"
      type="text"
      @keydown.enter="onSave"
    />
    <ul>
      <li
        v-for="items in itemsList"
        :key="items.id"
        class="d-flex j-between box"
      >
        <span>{{ items.title }}</span>
        <button @click="itemDetele(items)">sil</button>
      </li>
    </ul>

    <div>{{ itemsCount }}</div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      itemsList: [],
    };
  },
  mounted() {
    axios.get("http://localhost:3000/items").then((items_reponse) => {
      this.itemsList = items_reponse.data || [];
    });
  },
  methods: {
    onSave(e) {
      var saveObject = {
        title: e.target.value,
        create_at: new Date(),
        completed: false,
      };
      axios
        .post("http://localhost:3000/items ", saveObject)
        .then((save_reponse) => {
          this.itemsList.push(save_reponse.data);
        });
  
    },
    itemDetele(items) {
      axios
        .delete(`http://localhost:3000/items/${items.id}`)
        .then(delete_response =>{
          this.itemsList = this.itemsList.filter(i => i != items)
        });
    },
  },
  computed: {
    itemsCount() {
      return this.itemsList.length;
    },
  },
};
</script> 