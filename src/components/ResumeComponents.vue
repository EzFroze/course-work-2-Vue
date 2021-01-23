<template>
  <div v-for="comp in formComponents" :key="comp.id" class="component">
    <component :is="comp.type" :text="comp.text" />
    <div class="delete">
      <button @click="$emit('deleteComponent', comp.id)">Удалить</button>
    </div>
  </div>
</template>

<script>
import Avatar from "@/components/Avatar";
import Text from "@/components/Text";
import Title from "@/components/Title";
import Subtitle from "@/components/Subtitle";

export default {
  name: "ResumeComponents",
  components: {
    Subtitle,
    Title,
    Text,
    Avatar
  },
  emits: ["deleteComponent"],
  props: {
    formComponents: Array
  }
};
</script>

<style scoped>
.component {
  position: relative;
}

.delete {
  position: absolute;
  right: 0;
  top: 10px;
}
</style>
