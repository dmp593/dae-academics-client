<template>
  <b-container>
    <h4>Student Details:</h4>
    <p>Username: {{ student.username }}</p>
    <p>Name: {{ student.name }}</p>
    <p>Email: {{ student.email }}</p>
    <p>Course: {{ student.courseName }}</p>
    <h4>Subjects enrolled:</h4>
    <b-table
      v-if="subjectsEnrolled.length"
      striped
      over
      :items="subjectsEnrolled"
      :fields="subjectFields"
    >
      <template v-slot:cell(actions)="row">
        <button class="btn btn-danger" @click="unroll(row)">UNROLL</button>
      </template>
    </b-table>
    <p v-else>No subjects enrolled.</p>
    <h4>Subjects unrolled:</h4>
    <b-table
      v-if="subjectsUnrolled.length"
      striped
      over
      :items="subjectsUnrolled"
      :fields="subjectFields"
    >
      <template v-slot:cell(actions)="row">
        <button class="btn btn-primary" @click="enroll(row)">ENROLL</button>
      </template>
    </b-table>
    <p v-else>No subjects unrolled.</p>
    <nuxt-link to="/students">Back</nuxt-link>

    &nbsp;
    <nuxt-link to="/students">Go back</nuxt-link>
    &nbsp;
    <nuxt-link :to="`/students/${this.username}/send-email`">Send e-mail</nuxt-link
    >
  </b-container>
</template>
<script>
export default {
  data() {
    return {
      student: {},
      subjectsEnrolled: [],
      subjectsUnrolled: [],
      subjectFields: [
        "code",
        "name",
        "courseCode",
        "courseYear",
        "scholarYear",
        "actions",
      ],
    };
  },
  computed: {
    username() {
      return this.$route.params.username;
    },
  },
  created() {
    this.$axios
      .$get(`/api/students/${this.username}`)
      .then((student) => (this.student = student || {}))
      .then(() => this.$axios.$get(`/api/students/${this.username}/subjects`))
      .then((subjectsEnrolled) => (this.subjectsEnrolled = subjectsEnrolled))
      .then(() =>
        this.$axios.$get(`/api/students/${this.username}/subjects/unrolled`)
      )
      .then((subjectsUnrolled) => (this.subjectsUnrolled = subjectsUnrolled));
  },
  methods: {
    enroll(row) {
      console.log(row);
      const { index, item } = row;
      this.$axios
        .$patch(`/api/students/${this.username}/subjects/${item.code}/enroll`)
        .then(() => {
          this.subjectsUnrolled.splice(index, 1);
          this.subjectsEnrolled.push(item);
        });
    },
    unroll(row) {
      console.log(row);
      const { index, item } = row;
      this.$axios
        .$patch(`/api/students/${this.username}/subjects/${item.code}/unroll`)
        .then(() => {
          this.subjectsEnrolled.splice(index, 1);
          this.subjectsUnrolled.push(item);
        });
    },
  },
};
</script>
