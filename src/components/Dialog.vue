<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template v-slot:activator="{ on, attrs }">
      <div class="tt" v-bind="attrs" v-on="on">
        <slot></slot>
      </div>
    </template>
    <v-card>
      <v-card-title>
        <span class="text-h5">Edit</span>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-row>
              <v-col
                v-for="(item, ind) in headers.filter(
                  (h) => h.text !== 'Actions'
                )"
                :key="ind"
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  v-if="item.type === String"
                  :disabled="!item.editable"
                  :label="item.text"
                  v-model="editedItem[item.value]"
                  :rules="[numberRule]"
                ></v-text-field>
                <v-text-field
                  v-if="item.type === Number"
                  :disabled="!item.editable"
                  :label="item.text"
                  v-model="editedItem[item.value]"
                ></v-text-field>
                <v-checkbox
                  v-if="item.type === Boolean"
                  :disabled="!item.editable"
                  :label="item.text"
                  v-model="editedItem[item.value]"
                >
                </v-checkbox>
                <v-menu
                  v-if="item.type === Date"
                  ref="menu"
                  v-model="menu"
                  :close-on-content-click="false"
                  :return-value.sync="editedItem[item.value]"
                  transition="scale-transition"
                  offset-y
                  min-width="auto"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="editedItem[item.value]"
                      :label="item.text"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker v-model="date" no-title scrollable>
                    <v-spacer></v-spacer>
                    <v-btn text color="primary" @click="menu = false">
                      Cancel
                    </v-btn>
                    <v-btn text color="primary" @click="$refs.menu[0].save(date)">
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-menu>
              </v-col>
            </v-row>
          </v-form>
        </v-container>
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
  props: ["item", "headers"],
  data: () => ({
    date: null,
    menu: false,
    valid: false,
    dialog: false,
    editedItem: {},
    numberRule: function (v) {
      return /^\d+$/.test(v);
    },
  }),
  watch: {
    dialog: function (nVal) {
      if (nVal) this.init();
    },
  },
  updated: function () {
    if (this.dialog) {
      this.$refs.form.validate();
    }
    console.log(this.item)
  },
  methods: {
    log: function (tt) {
      tt;
    },
    init: function () {
      this.editedItem = {
        ...this.item,
      };
      console.log(this.item)
    },
    editItem: function () {
      // @NOTE: посмотри как прокидывать методы и пропсы через много объектов
      if (this.validate()) {
        this.$emit("editItem", this.editedItem);
        this.dialog = false;
      }
    },
    validate() {
      return this.valid;
    },
  },
};
</script>

<style scoped>
.tt {
  display: inline;
}
</style>