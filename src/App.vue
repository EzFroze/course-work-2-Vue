<template>
  <div class="container column">
    <form-component @addComponent="addComponent" />
    <div class="card card-w70">
      <resume-components
        :form-components="formComponents"
        @deleteComponent="deleteComponent"
      />
    </div>
  </div>
  <comments :comments="comments" @download="downloadComments" />
  <loader v-if="isLoading" />
</template>

<script>
import Loader from "@/components/Loader";
import Comments from "@/components/Comments";
import ResumeComponents from "@/components/ResumeComponents";
import FormComponent from "@/components/Form";

export default {
  components: {
    ResumeComponents,
    Comments,
    Loader,
    FormComponent
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
      const response = await fetch(process.env.VUE_APP_COMMENTS_URL);
      this.comments = await response.json();
      this.isLoading = false;
    },
    async addComponent(text, type) {
      const data = {
        type,
        text
      };
      const response = await fetch(
        `${process.env.VUE_APP_FIREBASE_URL}/components.json`,
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
        `${process.env.VUE_APP_FIREBASE_URL}/components.json`,
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
      await fetch(`${process.env.VUE_APP_FIREBASE_URL}/components/${id}.json`, {
        method: "DELETE",
        headers: {
          "Content-type": "application/json"
        }
      });
      this.formComponents = this.formComponents.filter(c => c.id !== id);
    }
  },
  mounted() {
    this.getComponents();
  }
};
</script>
