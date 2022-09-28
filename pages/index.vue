<template>
    <b-container>
        <!-- try to remove :fields=”fields” to see the magic -->
        <b-table striped over :items="students" :fields="fields" />
    </b-container>
</template>

<script>
export default {
    data () {
        return {
            fields: ['username', 'name', 'email'],
            students: []
        }
    },
    created () {
        this.$axios.$get('/api/students')
            .then((students) => { this.students = students })
    }
}
</script>
