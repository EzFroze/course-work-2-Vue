<template>
  <div class="avatar">
    <img :src="text" alt="myPhoto" />
  </div>
</template>

<script>
export default {
  name: "AppAvatar",
  props: {
    text: String
  }
};
</script>

<style>
.avatar {
  display: flex;
  justify-content: center !important;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
