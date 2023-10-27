<template>
  <v-app>
    <v-container>
        <v-row >
            <v-col cols="12" class="text-center"> <h1>Age Filter</h1></v-col>
        </v-row>
      <v-row>
        <v-col cols="12">
          <v-btn @click="addNewCard" color="primary" class="d-flex float-right"
            >Add New Card</v-btn
          >
          <v-text-field
            v-model="search"
            label="Search by Name"
            style="width: 300px"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col v-for="(range, index) in ageRanges" :key="index" cols="3">
          <v-row align="center">
            <v-col cols="8">
              <h2>{{ range.label }}</h2>
            </v-col>
            <v-col cols="4">
              <v-btn @click="toggleSort()" color="secondary">Sort</v-btn>
            </v-col>
          </v-row>

          <v-divider></v-divider>

          <v-row dense>
            <v-col
              v-for="(item, itemIndex) in getItemsInRange(
                range.min,
                range.max,
                range.sortAscending
              )"
              :key="itemIndex"
              cols="12"
            >
              <v-card
                v-if="matchesSearch(item.name)"
                class="card-style"
                :color="getRandomColor()"
                dark
              >
                <v-card-title class="headline">{{ item.name }}</v-card-title>
                <v-card-text>
                  <div>Email: {{ item.email }}</div>
                  <div>Age: {{ item.age }}</div>
                  <div>Phone Number: {{ item.phoneNumber }}</div>
                </v-card-text>
                <v-card-actions>
                  <v-btn
                    @click="editItem(item)"
                    color="white"
                    class="bg-primary"
                    >Edit</v-btn
                  >
                  <v-btn @click="deleteItem(item)" color="error" class="bg-red"
                    >Delete</v-btn
                  >
                </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
      </v-row>

      <v-dialog v-model="editDialog" max-width="600">
        <v-card>
          <v-card-title class="headline">Edit User</v-card-title>
          <v-card-text>
            <v-form>
              <v-row>
                <v-col cols="6"
                  ><v-text-field
                    v-model="editedItem.name"
                    label="Name"
                  ></v-text-field
                ></v-col>
                <v-col cols="6"
                  ><v-text-field
                    v-model="editedItem.email"
                    label="Email"
                  ></v-text-field
                ></v-col>
              </v-row>
              <v-row>
                <v-col cols="6">
                  <v-text-field
                    v-model="editedItem.age"
                    label="Age"
                  ></v-text-field
                ></v-col>
                <v-col cols="6"
                  ><v-text-field
                    v-model="editedItem.phoneNumber"
                    label="Phone Number"
                  ></v-text-field
                ></v-col>
              </v-row>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn @click="saveEditedItem" color="primary">Save</v-btn>
            <v-btn @click="cancelEdit" color="error">Cancel</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-dialog v-model="addDialog" max-width="600">
        <v-card>
          <v-card-title class="headline">Add User</v-card-title>
          <v-card-text>
            <v-form>
              <v-row>
                <v-col cols="6"
                  ><v-text-field
                    v-model="newItem.name"
                    label="Name"
                  ></v-text-field
                ></v-col>
                <v-col cols="6"
                  ><v-text-field
                    v-model="newItem.email"
                    label="Email"
                  ></v-text-field
                ></v-col>
              </v-row>
              <v-row>
                <v-col cols="6">
                  <v-text-field v-model="newItem.age" label="Age"></v-text-field
                ></v-col>
                <v-col cols="6"
                  ><v-text-field
                    v-model="newItem.phoneNumber"
                    label="Phone Number"
                  ></v-text-field
                ></v-col>
              </v-row>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn @click="addNewCardItem" color="primary">Add</v-btn>
            <v-btn @click="cancelAdd" color="error">Cancel</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-container>
  </v-app>
</template>
<script>
export default {
  data() {
    return {
      items: [
        {
          id: 12222,
          name: "ajay",
          age: 18,
          email: "ajay@gmail.com",
          phone: 12236363,
        },
        {
          id: 12242,
          name: "vijay",
          age: 16,
          email: "vijay@gmail.com",
          phone: 122556363,
        },
        {
          id: 12223,
          name: "balu",
          age: 12,
          email: "balu@gmail.com",
          phone: 12235363,
        },
        {
          id: 122282243,
          name: "kiran",
          age: 14,
          email: "balu@gmail.com",
          phone: 12235363,
        },
        {
          id: 1222578553,
          name: "gopal",
          age: 12,
          email: "balu@gmail.com",
          phone: 12235363,
        },
        {
          id: 11222,
          name: "siri",
          age: 35,
          email: "test2mail@gmail.com",
          phone: 12236363,
        },
        {
          id: 112258542,
          name: "satish",
          age: 29,
          email: "test2mail@gmail.com",
          phone: 12236363,
        },
        {
          id: 1122558228542,
          name: "ramu",
          age: 31,
          email: "test2mail@gmail.com",
          phone: 12236363,
        },
        {
          id: 1122575258542,
          name: "gopi",
          age: 29,
          email: "test2mail@gmail.com",
          phone: 12236363,
        },

        {
          id: 18242,
          name: "naruto",
          age: 46,
          email: "test2mail@gmail.com",
          phone: 122556363,
        },
        {
          id: 3988242,
          name: "vikranth",
          age: 46,
          email: "test2mail@gmail.com",
          phone: 122556363,
        },
        {
          id: 18457242,
          name: "ajith",
          age: 51,
          email: "test2mail@gmail.com",
          phone: 122556363,
        },
        {
          id: 1858755242,
          name: "sarada",
          age: 465,
          email: "test2mail@gmail.com",
          phone: 122556363,
        },
        {
          id: 182525242,
          name: "ben",
          age: 66,
          email: "test2mail@gmail.com",
          phone: 1225286363,
        },
        {
          id: 19223,
          name: "anand",
          age: 19,
          email: "test2mail@gmail.com",
          phone: 12235363,
        },
        {
          id: 1922578523,
          name: "ravi",
          age: 25,
          email: "test2mail@gmail.com",
          phone: 12235363,
        },
        {
          id: 1925858523,
          name: "lucky",
          age: 21,
          email: "lucky@gmail.com",
          phone: 12235363,
        },
        {
          id: 1922785583,
          name: "vikram",
          age: 21,
          email: "test2mail@gmail.com",
          phone: 12235363,
        },
        {
          id: 19223,
          name: "ganesh",
          age: 24,
          email: "test2mail@gmail.com",
          phone: 12235363,
        },
      ],
      editedItem: {
        name: "",
        email: "",
        age: "",
        phoneNumber: "",
      },
      newItem: {
        name: "",
        email: "",
        age: "",
        phoneNumber: "",
      },
      editDialog: false,
      addDialog: false,
      selectedIndex: null,
      ageRanges: [
        { label: "1-18", min: 1, max: 18, sortAscending: null },
        { label: "19-25", min: 19, max: 25, sortAscending: null },
        { label: "26-45", min: 26, max: 45, sortAscending: null },
        { label: "45+", min: 46, max: Infinity, sortAscending: null },
      ],
      search: "",
    };
  },
  methods: {
    editItem(item) {
      this.selectedIndex = this.items.indexOf(item);
      this.editedItem = { ...item };
      this.editDialog = true;
    },
    saveEditedItem() {
      if (this.selectedIndex !== null) {
        this.items[this.selectedIndex] = { ...this.editedItem };
      }
      this.editDialog = false;
    },
    cancelEdit() {
      this.editDialog = false;
    },
    addNewCard() {
      this.addDialog = true;
    },
    addNewCardItem() {
      this.items.push({ ...this.newItem });
      this.addDialog = false;
    },
    cancelAdd() {
      this.addDialog = false;
    },
    deleteItem(item) {
      const index = this.items.indexOf(item);
      this.items.splice(index, 1);
    },
    getItemsInRange(min, max, sortAscending) {
      const filteredItems = this.items.filter(
        (item) => item.age >= min && item.age <= max
      );
      if (sortAscending) {
        return filteredItems.sort((a, b) => (a.name > b.name ? 1 : -1));
      } else {
        return filteredItems.sort((a, b) => (a.name < b.name ? 1 : -1));
      }
    },
    toggleSort() {
      this.ageRanges.map((obj) => (obj.sortAscending = !obj.sortAscending));
    },
    matchesSearch(name) {
      const search = this.search.trim().toLowerCase();
      return name.toLowerCase().includes(search);
    },
    getRandomColor() {
      let color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
      return color;
    },
  },
};
</script>

<style scoped>
.card-style {
  margin: 5px;
  padding: 10px;
  border-radius: 10px;
}
</style>
