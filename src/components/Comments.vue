<template>
  <div class="comments">
    <button
      v-if="comments.length > commentsToShow"
      @click="showMoreComments"
      class="link"
    >
      {{ comments.length - commentsToShow }} earlier comments
    </button>
    <transition-group name="list">
      <div
        v-for="index in commentsToShow"
        v-bind:key="index"
        class="comments-item"
      >
        <div v-if="index < comments.length">
          <Comment
            @delete-comment="
              $emit(
                'delete-comment',
                comments[comments.length - commentsToShow + index - 1].id
              )
            "
            :comment="comments[comments.length - commentsToShow + index - 1]"
          />
        </div>
      </div>
    </transition-group>
  </div>
</template>

<script>
import Comment from "./Comment.vue";

export default {
  name: "Comments",
  components: {
    Comment,
  },
  props: {
    comments: Array,
  },
  data() {
    return {
      commentsToShow: 4,
    };
  },
  methods: {
    showMoreComments() {
      const remainingComments =
        this.comments.length - this.$data.commentsToShow;
      if (remainingComments >= 20) {
        this.$data.commentsToShow += 20;
      } else {
        this.$data.commentsToShow = this.comments.length;
      }
    },
  },
  emits: ["delete-comment"],
};
</script>

<style scoped>
.comments {
  padding: 1.25rem 1rem 0.25rem 1rem;
}

.list-enter-active,
.list-leave-active {
  transition: all 1s;
}
</style>
