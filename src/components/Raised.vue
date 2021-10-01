<template>
  <v-card class="mx-auto my-12" width="600" height="600" id="card">
    <v-container grid-list-xs>
      <v-layout row wrap>
        <v-card-text>
          <form v-if="currentTicket">
            <div class="form-group row">
              <label class="col-sm-2 col-form-label">TicketId:</label>
              <div class="col-sm-10">
                <input
                  type="text"
                  v-model="currentTicket.id"
                  disabled
                  class="form-control-plaintext"
                />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-sm-2 col-form-label">Date:</label>
              <div class="col-sm-10">
                <input
                  type="text"
                  v-model="currentTicket.submitDate"
                  disabled
                  class="form-control-plaintext"
                />
              </div>
            </div>

            <div class="form-group row">
              <label class="col-sm-2 col-form-label">Category:</label>
              <div class="col-sm-10">
                <input
                  type="text"
                  v-model="currentTicket.category"
                  disabled
                  class="form-control-plaintext"
                />
              </div>
            </div>

            <div class="form-group row">
              <label class="col-sm-2 col-form-label">Summary:</label>
              <div class="col-sm-10">
                <input
                  type="text"
                  v-model="currentTicket.summary"
                  disabled
                  class="form-control-plaintext"
                />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-sm-2 col-form-label">Description:</label>
              <div class="col-sm-10">
                <input
                  type="text"
                  v-model="currentTicket.description"
                  disabled
                  class="form-control-plaintext"
                />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-sm-2 col-form-label">Attachment:</label>
              <div class="col-sm-10">
                <input type="text" disabled class="form-control-plaintext" />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-sm-2 col-form-label">Status:</label>
              <div class="col-sm-10">
                <input
                  type="text"
                  v-model="currentTicket.status"
                  disabled
                  class="form-control-plaintext"
                />
              </div>
            </div>
          </form>
        </v-card-text>
      </v-layout>
      <hr />

      <div class="text-center">
        <v-dialog v-model="dialog" width="500">
          <template v-slot:activator="{ on, attrs }">
            <v-btn class="btn" v-bind="attrs" v-on="on" primary outlined>
              Add Comment
            </v-btn>
          </template>
          <v-card>
            <v-card-text class="commentsCard">
              <br />
              <textarea
                type="textarea"
                placeholder="Comment here"
                cols="60"
                rows="9"
                class="area"
                v-model="comment.text"
                name="message"
              ></textarea>
              &nbsp;
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn type="submit" color="primary" @click="dialog = false"
                >Add</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
      <v-btn class="view" color="primary" @click="sendComment">Send</v-btn>
    </v-container>
  </v-card>
</template>

<script>
import AllTicketsDataService from "../service/AllTicketDataServices";

export default {
  name: "ViewTicket",

  components: {},
  data() {
    return {
      currentTicket: null,
      text: "",
      comment: {
        text: "",
        dialog: false,
      },
    };
  },
  updated: false,
  methods: {
    sendComment() {
      const data = {
        text: this.comment.text,
      };
      AllTicketsDataService.createComment(data)
        .then((response) => {
          this.comment.id = response.data.id;
          console.log(response.data);
          this.submitted = true;
        })
        .catch((e) => {
          console.log(e);
        });
    },

    onSend: function (message) {
      alert(message);
    },
    onClickEditTicket() {
      this.dialog = true;
    },

    getTicket(id) {
      AllTicketsDataService.get(id)
        .then((response) => {
          this.currentTicket = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  mounted() {
    this.message = "";
    this.getTicket(this.$route.params.id);
  },
};
</script>

<style>
form input {
  margin-left: 30px;
}
form label {
  font-weight: bold;
  color: black;
  font-size: 17px;
}
.btn {
  color: rgb(95, 191, 247);
  /* border: 1px solid rgb(7, 21, 148); */
}

textarea {
  width: 90%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
  margin-top: 6px;

  resize: vertical;
}
.view {
  float: right;
  right: 3.5rem;
  margin-bottom: 16px;
}
#card {
  border: 1px solid rgb(161, 161, 161);
}
</style>
