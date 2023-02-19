<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input type="text" v-model="title" required>
    <label>Description:</label>
    <textarea required v-model="desc"></textarea>
    <input type="checkbox" v-model="complete">
    <button>Edit Project</button>
  </form>
</template>

<script>
export default {
  name: "EditProjectView",
  props: ['id'],
  data() {
    return {
      title: '',
      desc: '',
      complete: null,
      uri: 'http://localhost:3000/projects/'+ this.id
    }
  },
  mounted() {
    fetch(this.uri).then(res => res.json())
        .then(data => {
          this.title = data.title
          this.desc = data.desc
          this.complete = data.complete
        })
  },
  methods: {
    handleSubmit() {
      let p = {
        title: this.title,
        desc: this.desc,
        complete: this.complete
      }
      fetch(this.uri, {
        method: 'PATCH',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify(p)})
          .then(() => this.$router.push('/'))
          .catch(err => console.log(err.message))
    }
  }
}
</script>

<style scoped>
  form {
   background: white;
   padding: 20px;
   border-radius: 10px;
 }
 label {
   display: block;
   color: #bbb;
   text-transform: uppercase;
   font-size: 14px;
   font-weight: bold;
   letter-spacing: 1px;
   margin: 20px 0 10px 0;
 }
 input {
   padding: 10px;
   border: 0;
   border-bottom: 1px solid #ddd;
   width: 100%;
   box-sizing: border-box;
 }
 textarea {
   border: 1px solid #ddd;
   padding: 10px;
   width: 100%;
   box-sizing: border-box;
 }
 form button {
   display: block;
   margin: 20px auto 0;
   background: #00ce89;
   color: white;
   padding: 10px;
   border: 0;
   border-radius: 6px;
   font-size: 16px;
 }
</style>