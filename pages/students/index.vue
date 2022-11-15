<template>
  <b-container>
    <b-table striped over :items="students" :fields="fields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/students/${row.item.username}`">Details</nuxt-link>
        <nuxt-link :to="`/students/${row.item.username}/send-email`">Send e-mail</nuxt-link>
      </template>
    </b-table>
    <nuxt-link to="/students/create">Create a New Student</nuxt-link>
    <br>
    <nuxt-link to="/">Back</nuxt-link>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      fields: ["username", "name", "email", "courseName", "actions"],
      students: [],
    };
  },
  created() {
    this.$axios.$get("/api/students?limit=1000").then(({data}) => {
      this.students = data;
    });
  },
};
</script>
