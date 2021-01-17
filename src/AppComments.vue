<template>
  <div class="container">
    <p>
      <button
        class="btn primary"
        @click="$emit('download')"
        v-if="!comments.length"
      >
        Загрузить комментарии
      </button>
    </p>
    <div class="card" v-if="comments.length">
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="comment in comments" :key="comment.id">
          <div>
            <p>
              <strong>
                {{ comment.email }}
              </strong>
            </p>
            <small>
              {{ comment.body }}
            </small>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppComments",
  emits: ["download"],
  props: {
    comments: Array
  }
};
</script>
