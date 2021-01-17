<template>
  <form
    class="card card-w30"
    @submit.prevent="addComponentEmit(textArea, selectedType)"
  >
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="selectedType">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model.trim="textArea"></textarea>
    </div>

    <button class="btn primary" :disabled="activateAddBtn">
      Добавить
    </button>
  </form>
</template>

<script>
export default {
  name: "AppForm",
  data() {
    return {
      selectedType: "title",
      textArea: ""
    };
  },
  methods: {
    addComponentEmit(textArea, selectedType) {
      this.$emit("addComponent", textArea, selectedType);
      this.textArea = "";
    }
  },
  computed: {
    activateAddBtn() {
      return this.textArea.length < 4;
    }
  }
};
</script>
