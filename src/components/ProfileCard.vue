<template>
  <div class="profile">
    <div class="card">
      <DoctorIcon class="avatar" />
      <div class="data">
        <div class="profile-content">
          <div class="flex-row justify-between">
            <strong>{{ profile.name }}</strong>
            <a :href="profile.email" class="email">{{ profile.email }}</a>
          </div>
          <div class="description">{{ profile.description }}</div>
        </div>
        <div class="likes">
          <span class="likes-icon" @click="$emit('likeHandler')">
            <span v-if="!profile.userHasLikedProfile">🤍</span>
            <span v-else>💚</span>
          </span>
          <span class="likes-value">{{ profile.likes }}</span>
        </div>
        <div class="dislikes likes" @click="$emit('dislikeHandler')">
          <div class="likes-icon">
            <span v-if="profile.userHasDislikedProfile">👎</span>
            <span v-else>👎🏻</span>
          </div>
        </div>
      </div>
    </div>
    <div class="comment">
      <input class="comment-input" placeholder="Write your comment..." />
    </div>
  </div>
</template>

<script>
import DoctorIcon from "./DoctorIcon";

export default {
  name: "ProfileCard",
  emits: ["likeHandler , dislikeHandler"],
  components: {
    DoctorIcon,
  },

  props: {
    profile: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style>
.card {
  display: flex;
  align-items: center;
  border: solid 1px rgb(172, 172, 172);
  border-radius: 16px;
  padding: 1rem;
  font-size: 14px;
  color: rgb(82, 82, 82);
  background-color: #fff;
  box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
}

.avatar {
  width: 64px;
  min-width: 64px;
  height: 64px;
  border: 1px solid rgb(172, 172, 172);
  border-radius: 100%;
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.25), 0 5px 5px rgba(0, 0, 0, 0.22);
}

.data {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 100%;
}

.profile {
  background-color: rgba(140, 255, 226, 0.097);
  padding: 0.5rem;
  border-radius: 0.75rem;
  box-shadow: 2px 2px 5px rgb(46, 120, 101);
}

.profile-content {
  display: flex;
  flex-direction: column;
  text-align: left;
  margin-left: 15px;
}

.email {
  margin-left: 15px;
  text-decoration: none;
  color: rgb(8, 130, 89);
  font-weight: bold;
}

.description {
  margin-top: 5px;
  font-size: 0.8rem;
  color: rgb(124, 177, 143);
  font-weight: bold;
}

.likes {
  color: rgb(76, 202, 114);
  margin-top: 10px;
  margin-left: 15px;
}

.likes-value {
  margin-left: 5px;
}

.likes-icon {
  cursor: pointer;
}

.comment {
  display: flex;
  width: 100%;
}

.comment-input {
  width: 100%;
  border-radius: 12px;
  outline: none;
  border: none;
  padding: 0.5rem 1rem;
  margin-top: 0.5rem;
}
</style>
