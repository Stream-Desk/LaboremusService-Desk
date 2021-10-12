<template>
  <div>
    <div class="mx-3 my-6" width="200" id="card">
      <v-container grid-list-xs>
        <v-card>
          <v-btn text color="blue" class="backbtn" href="/table">Back</v-btn>
          <v-card-text>
            <form v-if="currentTicket">
              <div class="form-group row">
                <label class="col-sm-2 col-form-label">TicketId:</label>
                <div class="col-sm-10">
                  {{ currentTicket.id }}
                </div>
              </div>
              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Priority:</label>
                <div class="col-sm-10">
                  {{ currentTicket.priority }}
                </div>
              </div>

              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Requester:</label>
                <div class="col-sm-10">
                  {{ currentTicket.name }}
                </div>
              </div>

              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Category:</label>
                <div class="col-sm-10">
                  {{ currentTicket.category }}
                </div>
              </div>

              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Summary:</label>
                <div class="col-sm-10">
                  {{ currentTicket.summary }}
                </div>
              </div>

              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Date:</label>
                <div class="col-sm-10">
                  {{ currentTicket.submitDate }}
                </div>
              </div>

              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Attachment:</label>
                <div class="col-sm-10">
                  {{ currentTicket.attachment }}
                </div>
              </div>

              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Status:</label>
                <div class="col-sm-10">
                  {{ currentTicket.status }}
                </div>
              </div>
            </form>
          </v-card-text>
          <v-divider></v-divider>
          <comments></comments>

          <v-btn class="view" small color="primary" @click="sendComment">
            <span class="px-5">Send</span></v-btn
          >
        </v-card>
      </v-container>
    </div>
  </div>
</template>

<script>
import AllTicketsDataService from "../service/AllTicketDataServices";
import Comments from "../components/Comments.vue";

export default {
  name: "Raised",

  components: {
    Comments,
  },
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
  margin-top: -8%;
}
/* #card {
  border: 1px solid rgb(161, 161, 161);
} */
.backbtn {
  margin-top: 25px;
  text-transform: capitalize;
  margin-inline-start: 10px;
}
.divider {
  margin-left: 13%;
}
</style>
