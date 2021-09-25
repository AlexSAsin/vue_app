<template>
  <div>
    <vuetify-datatable
      :posts="posts"
      :headers="headers"
      sort-by="ID"
      @deleteItem="deleteItem"
      @editItem="editItem"
    ></vuetify-datatable>
  </div>
</template>

<script>
import VuetifyDatatable from "./components/VuetifyDatatable";
// import Dialog from "./components/Dialog";
export default {
  name: "App",
  components: {
    VuetifyDatatable,
    // Dialog,
  },
  data: () => ({
    posts: [
      {
        id: 1,
        name: "Корпус 1",
        parking_available: true,
        actions: "",
      },
      {
        id: 2,
        name: "Корпус 2",
        parking_available: false,
        actions: "",
      },
      {
        id: 3,
        name: "Корпус 3",
        parking_available: false,
        actions: "",
      },
    ],
    headers: [
      { text: "ID", value: "id", sortable: false, editable: false },

      { text: "Name", value: "name", sortable: false, editable: true },

      {
        text: "Parking Available",
        value: "parking_available",
        sortable: false,
         editable: true
      },
      { text: "Actions", value: "actions", sortable: false, editable: true },
    ],
  }),
  methods: {
    deleteItem(id) {
      this.posts = this.posts.filter((p) => p.id !== id);
    },
    editItem(newItem) {
      console.log(newItem);
      this.posts = this.posts.map((p) => (p.id !== newItem.id ? p : newItem));
    },
  },
};
</script>