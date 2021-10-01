<template>
  <v-app>
    <v-app-bar app color="" dark class="bar">
      <div class="d-flex align-center">
        <v-toolbar-title class="title"
          >Stream|<span>Desk</span></v-toolbar-title
        >
      </div>

      <v-spacer></v-spacer>
      <v-btn icon class="btn"><i class="fas fa-user"></i></v-btn>
      <v-btn icon class="btn"><i class="fas fa-bell"></i></v-btn>
      <email></email>

      <v-btn depressed class="button" small color="" href="">Login</v-btn>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
import Email from "../src/components/Email.vue";

export default {
  name: "App",
  components: {
    Email,
  },
  data: () => ({
    //
  }),
};
</script>

<style scoped>
.bar .title {
  color: rgb(250, 246, 20);
  font-size: 25px;
  text-transform: uppercase;
  font-weight: 800;
  list-style-type: none;
}
.bar .title span {
  color: #fff;
}
.btn {
  margin: 0 5px;
  font-size: 16px;
  width: 10px;
  height: 10px;
  line-height: 20px;
}
.button {
  border: 1px solid white;
}
.btns {
  border: 1px solid white;
  margin: 0 8px;
}
</style>
