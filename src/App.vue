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
import axios from "axios";

const dateRegex =
  /^\d{4}-[01]\d-[0-3]\dT[0-2]\d:[0-5]\d:[0-5]\d\.\d+([+-][0-2]\d:[0-5]\d|Z)$/;

// import Dialog from "./components/Dialog";
export default {
  name: "App",
  components: {
    VuetifyDatatable,
    // Dialog,
  },
  data: () => ({
    posts: [],
    headers: [
      {
        text: "ID",
        value: "id",
        sortable: false,
        editable: false,
        type: Number,
        ignoreNew: true
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
    async deleteItem(id) {
      this.posts = this.posts.filter((p) => p.id !== id);
      await axios.delete(`http://localhost:3000/posts/${id}`)
             .then(response => {
                 console.log(response);
             });

    },
    async editItem(newItem, id) {
      try {
        await axios.put(`http://localhost:3000/posts/${id}`, newItem);
        this.posts = this.posts.map((p) => (p.id !== newItem.id ? p : newItem));
      } catch (e) {
        this.errors.push(e);
      }
    },
    async newItem(newItem) {
      try {
        let response = await axios.post(`http://localhost:3000/posts/`, {
          ...newItem,
        });
        this.posts = [...this.posts, {...response.data, inOperationDate: response.data.inOperationDate.split("T")[0]}];
      } catch (e) {
        this.errors.push(e);
      }
    },
  },
  created() {
    axios.get("http://localhost:3000/posts/").then((response) => {
      this.posts = response.data.map((p) => {
        if (dateRegex.test(p.inOperationDate)) {
          p = { ...p, inOperationDate: p.inOperationDate.split("T")[0] };
        }
        return p;
      });
    });
  },
};
</script>


<style scoped>
#app {
  background-color: #eee;
}
</style>