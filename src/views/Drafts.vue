<template>
  <div>
    Drafts
    <ul v-if="!err">
      <li v-for="draft in drafts" :key="draft.id" @click="loadDraft(draft)">
        <div>{{ draft.id }}</div>
        <div>{{ draft.timestamps.firstCreated }}</div>
        <div>{{ draft.timestamps.lastUpdated }}</div>
      </li>
    </ul>
    <v-alert v-if="err" type="error">
      {{ err }}
    </v-alert>
  </div>
</template>

<script>
export default {
  name: "Drafts",
  data: () => ({
    drafts: [],
    err: null
  }),
  methods: {
    loadDraft(draft) {
      this.$store.commit("loadDraft", draft);
      this.$router.push("/");
    }
  },
  async created() {
    await fetch("http://localhost:3000/drafts")
      .then(response => response.json())
      .then(data => (this.drafts = data))
      .catch(() => (this.err = "Something went wrong with the server"));
  }
};
</script>

<style scoped>
li {
  cursor: pointer;
  display: flex;
  justify-content: space-between;
}
</style>
