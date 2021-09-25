<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template v-slot:activator="{ on, attrs }">
      <div class="tt" v-bind="attrs" v-on="on">
        <slot> </slot>
      </div>
    </template>
    <v-card>
      <v-card-title>
        <span class="text-h5">User Profile</span>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-row>
            <!-- @TODO: сделать эти поля с помощью цикла от headers -->
            <v-col cols="12" sm="6" md="4">
              <v-text-field label="Legal first name*" required></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <!-- @NOTE: прочитай v-model -->
              <v-text-field
                label="Legal middle name"
                hint="example of helper text only on focus"
                v-model="editedItem.name"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
        <small>*indicates required field</small>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="dialog = false">
          Close
        </v-btn>
        <v-btn color="blue darken-1" text @click="editItem()"> Save </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ["item", "header"],
  data: () => ({
    dialog: false,
    editedItem: {},
  }),
  watch: {
    dialog: function (nVal) {
      if (nVal) this.init();
    },
  },
  methods: {
    init: function () {
      this.editedItem = {
        ...this.item,
      };
    },
    editItem: function () {
      // @NOTE: посмотри как прокидывать методы и пропсы через много объектов
      this.$emit("editItem", this.editedItem);
      this.dialog = false;
    },
  },
};
</script>

<style scoped>
.tt {
  display: inline;
}
</style>