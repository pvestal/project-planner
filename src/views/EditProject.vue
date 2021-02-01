<template>
  <div>
    <h1>Edit Project</h1>
    <form @submit.prevent="handleSubmit">
      <label>Title:</label>
      <input type="text" v-model="title" required />
      <label>Details:</label>
      <textarea v-model="details" required></textarea>
      <input type="checkbox" v-model="complete" />
      <button>Update Project</button>
    </form>
  </div>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      complete: '',
      uri: 'http://localhost:3000/projects/' + this.id
    }
  },
  mounted() {
    fetch(this.uri)
    .then(res => res.json())
    .then(data => {
      this.title = data.title,
      this.details = data.details,
      this.complete = data.complete
    })
  },
   methods: {
     handleSubmit() {
        fetch(this.uri, {
          method: 'PATCH',
          headers: {'Content-Type': 'application/json'},
          body: JSON.stringify({title: this.title, details: this.details})
        })
          .then(() => this.$router.push('/'))
          .catch(err => alert("Error: ", err.message))
     }
   }
};
</script>

<style>
</style>