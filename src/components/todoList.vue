<template>
  <div class="main">
    <v-row class="justify-center">
      <v-col cols="12" class="title text-center">
        <h1>TASKS</h1>
      </v-col>
      <v-col cols="12" class="add_List_bnt text-center">
        <v-btn
          @click="dialog = !dialog"
          class="mx-2"
          small
          fab
          dark
          color="indigo"
        >
          <v-icon dark>mdi-plus</v-icon>
        </v-btn>
        <span> ADD NEW A TASK</span>
      </v-col>

      <v-col cols="12" class="incomplete text-center">
        <div
          class="d-flex align-center justify-center"
          v-for="item in todoList"
          :key="item.id"
        >
          <v-checkbox @click="checkComplete(item)"></v-checkbox>
          <v-btn text @click="dialog_detail = !dialog_detail">{{ item.title }}</v-btn>
          <v-btn
            class="mx-2"
            small
            fab
            dark
            color="red"
            @click="deleteData(item.id)"
          >
            <v-icon dark>mdi-delete</v-icon>
          </v-btn>
        </div>

        <div v-if="completedList.length > 0">
          <v-btn
            class="ma-2"
            outlined
            color="indigo"
            @click="showCompleted = !showCompleted"
            >Show/Hide completed</v-btn
          >
        </div>        
      </v-col>

      <v-col cols="12" v-if="showCompleted">
        <div
          class="d-flex align-center justify-center "
          v-for="item in completedList"
          :key="item.id"
        >
          <v-checkbox v-model="item.id" disabled></v-checkbox>
          <v-btn
            text
            @click="dialog_detailCompleted = !dialog_detailCompleted"
            style="text-decoration: line-through;"
            >{{ item.title }}</v-btn
          >
          <v-btn
            @click="deleteDataCompleted(item.id)"
            class="mx-2"
            small
            fab
            dark
            color="red"
          >
            <v-icon dark>mdi-delete</v-icon>
          </v-btn>
        </div>
        <div class="text-center" v-if="completedList.length > 0">
          <v-btn
            @click="clearListCompleted()"
            class="ma-2"
            outlined
            color="indigo"
            >Delete All Completed</v-btn
          >
        </div>
      </v-col>

      <v-dialog v-model="dialog" width="500">
        <v-card>
          <v-card-title class="headline grey lighten-2">
            ADD TO-DO LIST
          </v-card-title>

          <div class="add_List px-4">
            <v-text-field
              label="Title"
              hide-details="auto"
              v-model="title"
            ></v-text-field>
            <v-textarea
              label="Detail"
              v-model="detail"
            ></v-textarea>
          </div>
          <v-divider></v-divider>

          <v-card-actions>
            <v-btn class="mr-4" @click="saveData()"
              ><v-icon class="ma-2" dark right>mdi-plus</v-icon>Add</v-btn
            >
            <v-btn @click="cancel()">
              <v-icon class="ma-2" dark right>mdi-window-close</v-icon>cancel
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-dialog v-model="dialog_detail" width="500">
        <v-card>
          <v-card-title class="headline grey lighten-2">
            Detail
          </v-card-title>

          <div class="add_List px-4">
            <v-text-field
              label="To-do Title"
              hide-details="auto"
              disabled
            ></v-text-field>
            <v-text-field label="To-do detail" disabled></v-text-field>
          </div>
          <v-divider></v-divider>

          <v-card-actions>
            <v-checkbox label="mark as completed"></v-checkbox>
            <v-btn class="mr-4"
              ><v-icon class="ma-2" dark right>mdi-minus</v-icon>Delete</v-btn
            >
            <v-btn @click="cancel()">
              <v-icon class="ma-2" dark right>mdi-window-close</v-icon>cancel
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      
      <v-dialog v-model="dialog_detailCompleted" width="500">
        <v-card>
          <v-card-title class="headline grey lighten-2">
            Detail
          </v-card-title>

          <div class="add_List px-4">
            <v-text-field
              label="To-do Title"
              hide-details="auto"
              disabled
            ></v-text-field>
            <v-text-field label="To-do detail" disabled></v-text-field>
          </div>
          <v-divider></v-divider>

          <v-card-actions>
            
            <v-btn class="mr-4"
              ><v-icon class="ma-2" dark right>mdi-minus</v-icon>Delete</v-btn
            >
            <v-btn @click="cancel()">
              <v-icon class="ma-2" dark right>mdi-window-close</v-icon>cancel
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
  </div>
</template>

<script>
export default {
  name: "todoList",
  data: () => {
    return {
      dialog: false,
      dialog_detail: false,
      showCompleted: false,
      dialog_detailCompleted: false,
      showBtn: false,
      title: "",
      detail: "",
      todoList: [],
      completedList: [],
    };
  },
  created() {
    let parseJson = localStorage.getItem("todoData");
    let parseJsonCompleted = localStorage.getItem("todoCompleted");

    if (parseJson) {
      this.todoList = JSON.parse(parseJson);
    }

    if (parseJsonCompleted) {
      this.completedList = JSON.parse(parseJsonCompleted);
    }

    console.log(this.todoList, this.completedList);
  },
  methods: {
    saveData() {
      let todoInfo = {
        id: Date.now(),
        title: this.title,
        detail: this.detail,
      };
      this.todoList.push(todoInfo);
      let getJson = JSON.stringify(this.todoList);
      localStorage.setItem("todoData", getJson);
      this.title = "";
      this.detail = "";

      console.log(this.todoList, getJson);
      this.dialog = false;
    },

    deleteData(id) {
      console.log("delete", id);
      let todoListUpdate = this.todoList;
      this.todoList = todoListUpdate.filter((todo) => todo.id !== id);
      let getJson = JSON.stringify(this.todoList);
      localStorage.setItem("todoData", getJson);
    },
    deleteDataCompleted(id) {
      let todoListUpdate = this.completedList;
      this.completedList = todoListUpdate.filter((todo) => todo.id !== id);
      let getJson = JSON.stringify(this.completedList);
      localStorage.setItem("todoCompleted", getJson);
    },
    checkComplete(item) {
      this.completedList.push(item);
      let getJson = JSON.stringify(this.completedList);
      localStorage.setItem("todoCompleted", getJson);
      this.deleteData(item.id);
    },
    clearListCompleted() {
      this.completedList = [];
      let getJson = JSON.stringify(this.completedList);
      localStorage.setItem("todoCompleted", getJson);
    },
    cancel() {
      this.dialog = false;
      this.dialog_detail = false;
      this.dialog_detailCompleted = false;
      this.title = "";
      this.detail = "";
    },
    showDetail() {
      //this.dialog_detail = !this.dialog_detail
      // let todoListUpdate = this.completedList;
    },
  },
};
</script>
<style scoped>

.main {
  max-width: 600px;
  margin: 30px auto 0;
  border: 1px solid gray;
  background-color: ivory;
  
}
</style>
