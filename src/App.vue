<template>
  <v-app>
    <v-main>
      <todoList />
    </v-main>
  </v-app>
</template>

<script>
import todoList from "./components/todoList";

export default {
  name: "App",

  components: {
    todoList,
  },

  data: () => ({
    //
  }),
};
</script>
