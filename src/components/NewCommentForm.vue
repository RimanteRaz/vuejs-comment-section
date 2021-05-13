<template>
  <form @submit="onSubmit" @reset="onReset" class="new-comment-form" action="">
    <input name="text" v-model="text" id="text" placeholder="Add a comment" />
    <button type="submit" class="btn-primary">Post</button>
    <button type="reset" class="btn-secondary">Cancel</button>
  </form>
</template>

<script>
export default {
  name: "NewCommentForm",
  data() {
    return {
      text: "",
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      if (!this.text) {
        alert("Please write your comment");
        return;
      }
      this.$emit("add-comment", this.text);
      this.text = "";
    },
    onReset() {
      this.text = "";
    },
  },
};
</script>

<style scoped>
form {
  background-color: #f3f3f8;
  padding: 1rem;
  width: inherit;
  border-end-start-radius: 5px;
  border-end-end-radius: 5px;
}

input {
  display: block;
  box-sizing: border-box;
  width: 100%;
  border: none;
  outline: none;
  background: none;
  border-bottom: 1.5px solid #868e95;
  transition: border-bottom 400ms ease-in-out;
  padding: 0.6rem;
  margin-bottom: 1rem;
  color: #4e4e64;
}

input:focus {
  border-bottom: 1.5px solid #3109bc;
  color: #2b2b4a;
}

button:nth-of-type(1) {
  margin-right: 5px;
}
</style>
