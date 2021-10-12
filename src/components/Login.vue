<template>
  <v-main class="d-flex justify-center align-center">
    <v-card width="370" height="330" class="mx-auto mt-9 pa-9" id="card">
      <div>
        <h5 class="text-center" id="title">Stream|<span>Desk</span></h5>
      </div>
      <v-form @submit.prevent="submitHandler" ref="form" v-model="valid">
        <v-card-text>
          <v-text-field
            v-model="username"
            :rules="usernameRules"
            type="name"
            label="Username"
            placeholder="Username"
            prepend-inner-icon="mdi-account"
          />

          <v-text-field
            label="Password"
            v-model="password"
            :rules="passwordRules"
            required
            placeholder="Password"
            :type="showPassword ? 'text' : 'password'"
            prepend-inner-icon="mdi-key"
            :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
            @click:append="showPassword = !showPassword"
          />
        </v-card-text>

        <v-card-actions class="justify-center">
          <v-btn type="submit" color="yellow" :disabled="!valid">
            <span class="px-8">Login</span>
          </v-btn>
        </v-card-actions>
      </v-form>
    </v-card>
  </v-main>
</template>

<script>
//import AllTicketDataService from "../service/AllTicketDataServices";
import axios from "axios";

export default {
  name: "Login",

  data() {
    return {
      valid: false,
      showPassword: false,
      password: "",
      passwordRules: [
        (v) => !!v || "Password is required",
        (v) => (v && v.length >= 6) || "Password must be 6 characters or more",
      ],
      username: "",
      usernameRules: [
        (v) => !!v || "username is required",
        (v) => (v && v.length >= 3) || "Password must be 3 characters or more",
      ],
    };
  },

  methods: {
    submitHandler() {
      this.$refs.form.validate();

      const formData = {
        userName: this.username,
        password: this.password,
      };
      axios
        .post(
          "https://streamdeskticketmicroservice.herokuapp.com/api/Users/authenticate",
          formData
        )
        .then((response) => {
          console.log(response, "Successfully login");
          this.$router.push("/table");
        })
        .catch((error) => {
          console.log(error, "Failed to login");
        });
    },
  },
};
</script>

<style scoped>
#card {
  border-radius: 10px;
  border: 1px solid rgb(201, 201, 201);
}
#title {
  color: rgb(253, 234, 59);
}
span {
  color: rgb(29, 29, 29);
}
</style>
