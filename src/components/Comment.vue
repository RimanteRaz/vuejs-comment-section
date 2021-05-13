<template>
  <div
    class="comment"
    v-on:mouseover="active = true"
    v-on:mouseleave="active = false"
  >
    <div class="div1">
      <div class="profile"><img :src="comment.picture" alt="" /></div>
    </div>
    <div class="div2 space-between">
      <div>
        <strong>{{ comment.username }}</strong>
      </div>
      <div class="muted-text">{{ formatTime(comment.timePosted) }}</div>
      <div>
        <button v-if="active" @click="onDelete(comment.id)" class="btn-danger">
          Delete
        </button>
      </div>
    </div>
    <div class="div3">
      <p>{{ comment.text }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Comment",
  props: { comment: Object },
  data() {
    return { active: false };
  },
  methods: {
    onDelete(id) {
      this.$emit("delete-comment", id);
    },
    formatTime(time) {
      let timeAbbrevation = "AM";
      let hours = new Date(time).getHours();
      if (hours > 12) {
        timeAbbrevation = "PM";
        hours = hours - 12;
      }
      let minutes = new Date(time).getMinutes().toString();
      if (minutes.length < 2) {
        minutes = "0" + minutes;
      }
      const timeForDisplay = `${hours}:${minutes} ${timeAbbrevation}`;
      return timeForDisplay;
    },
  },
};
</script>

<style scoped>
.comment {
  display: grid;
  background-color: #f4f4f6;
  grid-template-columns: 45px 1fr;
  grid-template-rows: 0.85rem 1fr;
  grid-column-gap: 15px;
  grid-row-gap: 0.4rem;
  margin: 1rem 0;
  padding: 0.8rem 0.5rem;
  border-radius: 10px;
}

.div1 {
  grid-area: 1 / 1 / 3 / 2;
}
.div2 {
  grid-area: 1 / 2 / 2 / 3;
}
.div3 {
  grid-area: 2 / 2 / 3 / 3;
}

.div2 div:nth-of-type(2) {
  margin-left: 12px;
  margin-right: auto;
}

.space-between {
  display: flex;
  justify-content: space-between;
}

p {
  margin: 0;
}

.profile img {
  width: 45px;
  height: 45px;
  border-radius: 30px;
}

.muted-text {
  color: #63637d;
}
</style>
