<template>
  <div class="container column">
    <app-form @addComponent="addComponent" />
    <div class="card card-w70">
      <div v-for="comp in formComponents" :key="comp.id" class="component">
        <component :is="comp.component" :text="comp.text" />
        <div class="delete">
          <button @click="deleteComponent(comp.id)">Удалить</button>
        </div>
      </div>
    </div>
  </div>
  <app-comments :comments="comments" @download="downloadComments" />
  <app-loader v-if="isLoading" />
</template>

<script>
import AppLoader from "@/AppLoader";
import AppComments from "@/AppComments";
import AppAvatar from "@/AppAvatar";
import AppText from "@/AppText";
import AppForm from "@/AppForm";
import AppTitle from "@/AppTitle";
import AppSubtitle from "@/AppSubtitle";

export default {
  components: {
    AppSubtitle,
    AppTitle,
    AppForm,
    AppText,
    AppAvatar,
    AppComments,
    AppLoader
  },
  data() {
    return {
      comments: [],
      isLoading: false,
      selectedType: "title",
      formComponents: []
    };
  },
  methods: {
    async downloadComments() {
      this.isLoading = true;
      const response = await fetch(
        "https://jsonplaceholder.typicode.com/comments?_limit=42"
      );
      this.comments = await response.json();
      this.isLoading = false;
    },
    async addComponent(text, type) {
      const component = `app-${type}`;
      const data = {
        component,
        text
      };
      const response = await fetch(
        "https://vue-with-http-8a60d-default-rtdb.firebaseio.com/components.json",
        {
          method: "POST",
          headers: {
            "Content-type": "application/json"
          },
          body: JSON.stringify(data)
        }
      );
      if (response.status === 200) {
        this.formComponents.push(data);
      }
    },
    async getComponents() {
      const response = await fetch(
        "https://vue-with-http-8a60d-default-rtdb.firebaseio.com/components.json",
        {
          method: "GET",
          headers: {
            "Content-type": "application/json"
          }
        }
      );
      const data = await response.json();
      if (data) {
        this.formComponents = Object.keys(data).map(key => {
          return {
            ...data[key],
            id: key
          };
        });
      }
    },
    async deleteComponent(id) {
      await fetch(
        `https://vue-with-http-8a60d-default-rtdb.firebaseio.com/components/${id}.json`,
        {
          method: "DELETE",
          headers: {
            "Content-type": "application/json"
          }
        }
      );
      this.formComponents = this.formComponents.filter(c => c.id !== id);
    }
  },
  mounted() {
    this.getComponents();
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
