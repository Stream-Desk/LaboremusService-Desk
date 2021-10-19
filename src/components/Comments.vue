<template>
  <div class="navbar-header">
    <h6>Comments</h6>
    <div class="comment" v-for="(comment, index) in comments" :key="index">
      <div>
        <div>
          <div class="reply">
            <v-avatar color="yellow">MM</v-avatar>
            <div class="alert" v-if="alert">
              <a href="#">May</a>
              {{ alert }}
            </div>
            <p>{{ comment.text }}</p>
          </div>
          <div class="crud">
            <v-icon my-3 @click="getComment(comments.id)" color="primary">mdi-pencil</v-icon>
            <v-icon my-3 @click="deleteTicket(comment.id)" color="red">mdi-delete</v-icon>
          </div>
        </div>
      </div>
      <div></div>
      <v-dialog
        v-model="dialog"
        max-width="800"
        transition="dialog-bottom-transition"
      >
        <v-card>
          <v-card-title class="text-h5"> Delete Comment </v-card-title>

          <v-card-text>
            Are you sure you want to delete this comment?
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>

            <v-btn color="primary darken-1"> Delete </v-btn>

            <v-btn color="primary darken-1" @click="dialog = false">
              Cancel
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>

    <div class="comment-form" v-if="!isHidden">
      <textarea
        type="text"
        v-model="comment.text"
        placeholder="Leave Comment here...  "
      ></textarea>

      <v-btn v-on:click="isHidden = false" @click="sendComment" small  color="primary">
       <span class="px-5">Send</span>
      </v-btn>
    </div>

    <!-- <v-btn small v-on:click="isHidden = !isHidden" color="primary" class="px-5">
      <v-icon my-2>mdi-comment</v-icon>
      Comment
    </v-btn> -->
  </div>
</template>

<script>
import CommentService from "../service/CommentDataService";
export default {
  data() {
    return {
      comments: [],
      currentComment: null,
      comment: {
        text: "",
        reply: "",
      },
      dialog: false,
      isHidden: false,
    };
  },
  created() {
    this.retrieveComment();
  },
  methods: {
    //Open dialog
    open() {
      this.dialog = true;
    },
    // @Send comment
    sendComment() {
      const data = {
        text: this.comment.text,
      };
      CommentService.createComment(data)
        .then((response) => {
          this.comment.id = response.data.id;
          console.log(response.data);
          this.submitted = true;
        })
        .catch((e) => {
          console.log(e);
        });
    },
    // @Get all comments
    retrieveComment(id) {
      var comments = comments;
      setInterval(() => {
        CommentService.getAll(id)
          .then((response) => {
            this.comments = response.data;
            this.alert = "added comment - just now";
            console.log(response.data);
          })
          .catch((e) => {
            console.log(e);
          });
      }, 10000);
    },
    // @Get comment by id
    getComment(id) {
      CommentService.get(id)
        .then((response) => {
          this.comments = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },
    // @Update comment by id:id

    // @Delete Comments

    deleteTicket(id) {
      CommentService.delete(id)
        .then(() => {
          this.dialog = false;
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
};
</script>

<style>
textarea[type="text"] {
  width: 98%;
  border: 1px solid #ccc;
  border-radius: 30px;
  height: 69px;
  font-family: Verdana, Helvetica, sans-serif;
  padding: 10px;
  margin-inline-start: 10px;
}
button {
  color: #ccc;
  border: 0;
  padding: 5px;
  cursor: pointer;
  margin-inline-start: 10px;
  margin-bottom: 10px;
}
.reply {
  border-left: 3px solid rgb(49, 49, 49);
  padding: 5px 5px;
  background: rgb(238, 242, 243);
  margin-inline-start: 10px;
  margin-right: 10px;
}
.crud {
  display: flex;
  position: relative;
  justify-content: right;
  bottom: 7rem;
  margin: 0 10px;
}
hr {
  border: 2px solid gray;
  margin-top: 2rem;
}
.navbar-header {
  margin: 2rem 0;
}
.comment {
  margin: 5px 0;
}
#content {
  display: flex;
  justify-content: center;
}
.title {
  display: flex;
  justify-content: center;
}
.alert {
  display: flex;
  position: relative;
  bottom: 2.5rem;
  margin: 0 4em;
}
a {
  padding: 0 10px;
}
h6 {
  margin-inline-start: 10px;
}
</style>
