<template>
  <div class="comment-section">
    <Comments @delete-comment="deleteComment" :comments="comments" />
    <NewCommentForm @add-comment="addComment" />
  </div>
</template>

<script>
import Comments from "./components/Comments.vue";
import NewCommentForm from "./components/NewCommentForm.vue";

export default {
  name: "App",
  components: {
    Comments,
    NewCommentForm,
  },
  data() {
    return {
      comments: [],
      currentUser: {
        username: "Current User",
        picture: "https://randomuser.me/api/portraits/thumb/men/67.jpg",
      },
    };
  },
  methods: {
    async addComment(submittedText) {
      const newComment = {
        username: this.currentUser.username,
        picture: this.currentUser.picture,
        timePosted: Date(),
        text: submittedText,
      };
      const res = await fetch("api/comments", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(newComment),
      });

      const data = await res.json();

      this.comments = [...this.comments, data];
    },
    async deleteComment(id) {
      const res = await fetch(`api/comments/${id}`, {
        method: "DELETE",
      });

      res.status == 200
        ? (this.comments = this.comments.filter((comment) => comment.id !== id))
        : alert("Error while deleting task");
    },
    async fetchComments() {
      const res = await fetch("api/comments");
      const data = await res.json();
      return data;
    },
  },
  async created() {
    this.comments = await this.fetchComments();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Rubik:wght@400;500;600&display=swap");

body {
  background-color: #d7d7e9;
  /* font-size: 1rem; */
}
#app {
  font-family: "Rubik", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2b2b4a;
  margin-top: 3rem;
  font-size: 0.85rem;
}

button {
  font-family: "Rubik", sans-serif;
  font-size: 0.85rem;
  font-weight: 400;
  outline: none;
  border: none;
  box-shadow: none;
  cursor: pointer;
}

input {
  font-family: "Rubik", sans-serif;
  font-size: 0.85rem;
  font-weight: 400;
}

.link {
  color: #4e4e64;
  background: none;
  padding: 0;
  transition: color 150ms ease-in;
}

.link:hover {
  color: #3109bc;
}

.btn-danger {
  display: block;
  box-sizing: border-box;
  padding: 0;
  background: none;
  color: #4e4e64;
  transition: color 150ms ease-in;
}

.btn-danger:hover {
  color: #b31625;
}

.btn-primary {
  box-sizing: border-box;
  padding: 7px 15px;
  background-color: #410ddb;
  color: white;
  border-radius: 5px;
  transition: background-color 150ms ease-in;
}

.btn-primary:hover {
  background-color: #24069d;
}

.btn-secondary {
  box-sizing: border-box;
  padding: 7px 16px;
  background-color: #d7d7e9;
  border-radius: 5px;
  transition: background-color 150ms ease-in;
}

.btn-secondary:hover {
  background-color: #c3c3de;
}

.comment-section {
  background-color: #ededf5;
  max-width: 50ch;
  margin: 0 auto;
  border-radius: 10px;
  box-shadow: 0px 2px 5px rgba(195, 195, 222, 0.6);
}
</style>
