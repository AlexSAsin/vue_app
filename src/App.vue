<template>
  <div>
    <vuetify-datatable
      :posts="posts"
      :headers="headers"
      sort-by="ID"
      @deleteItem="deleteItem"
      @editItem="editItem"
      @newItem="newItem"
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
        parkingAvailable: true,
        inOperationDate: new Date(
          Date.now() - new Date().getTimezoneOffset() * 60000
        )
          .toISOString()
          .split("T")[0],
        actions: "",
      },
      {
        id: 2,
        name: "Корпус 2",
        parkingAvailable: false,
        inOperationDate: new Date(
          Date.now() - new Date().getTimezoneOffset() * 60000
        )
          .toISOString()
          .split("T")[0],
        actions: "",
      },
    ],
    headers: [
      {
        text: "ID",
        value: "id",
        sortable: false,
        editable: false,
        type: Number,
      },

      {
        text: "Name",
        value: "name",
        sortable: false,
        editable: true,
        type: String,
      },
      {
        text: "Floors Count",
        value: "floorsCount",
        sortable: true,
        editable: true,
        type: Number,
      },
      {
        text: "Lot Type",
        value: "lotType",
        sortable: true,
        editable: true,
        type: Number,
      },
      {
        text: "In Operation Date",
        value: "inOperationDate",
        sortable: false,
        editable: true,
        type: Date,
      },
      {
        text: "Parking Available",
        value: "parkingAvailable",
        sortable: true,
        editable: true,
        type: Boolean,
      },
      {
        text: "Parking Count",
        value: "parkingCount",
        sortable: true,
        editable: true,
        type: Number,
      },
      {
        text: "Constructive Type",
        value: "constructiveType",
        sortable: true,
        editable: true,
        type: String,
      },
      {
        text: "District",
        value: "district",
        sortable: true,
        editable: true,
        type: String,
      },
      {
        text: "City",
        value: "city",
        sortable: true,
        editable: true,
        type: String,
      },
      {
        text: "Actions",
        value: "actions",
        sortable: false,
        editable: true,
        type: String,
      },
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
    newItem(newItem) {
      this.posts = [...this.posts, newItem];
    },
  },
};
</script>


<style scoped>
#app {
  background-color: #eee;
}
</style>