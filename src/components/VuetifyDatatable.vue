<template>
  <v-app>
    <v-main>
      <v-container>
        <h2>Dombook</h2>
        <v-row>
          <v-col md="2">
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
          </v-col>
          <v-col md="2" offset="8">
            <Dialog
              :item="{id: posts.length+1,parkingAvailable: false}"
              :headers="headers"
              @editItem="(newItem) => $emit('newItem', newItem)"
            >
              <v-btn dark width="80%"> ADD </v-btn>
            </Dialog>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-data-table
              :headers="headers"
              :items="posts"
              :items-per-page="5"
              :search="search"
              class="elevation-1"
            >
              <template v-slot:[`item.actions`]="{ item }">
                <v-icon small @click="$emit('deleteItem', item.id)"
                  >mdi-delete</v-icon
                >
                <!-- @TODO: исправить костыли с emit -->
                <Dialog
                  :item="item"
                  :headers="headers"
                  @editItem="(newItem) => $emit('editItem', newItem)"
                >
                  <v-icon small>mdi-pencil</v-icon>
                </Dialog>
              </template>
            </v-data-table>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Dialog from "./Dialog.vue";
export default {
  components: { Dialog },
  props: ["posts", "headers"],
  data: () => ({
    search: "",
  }),
  methods: {},
};
</script>