<template>
  <div class="container py-5">
    <form @submit.stop.prevent="handleSubmit">
      <div class="form-group">
        <label for="name">Name</label>
        <input
          v-model="profile.name"
          id="name"
          type="text"
          name="name"
          class="form-control"
          placeholder="Enter Name"
          required
        />
      </div>

      <div class="form-group">
        <label for="image">Image</label>
        <img
          v-if="profile.image"
          :src="profile.image"
          class="d-block img-thumbnail mb-3"
          width="200"
          height="200"
        />
        <input
          id="image"
          type="file"
          name="image"
          accept="image/*"
          class="form-control-file"
          @change="handleFileChange"
        />
      </div>

      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
const dummyUser = {
  profile: {
    id: 2,
    name: 'user1',
    image: 'https://i.imgur.com/PhcKzNf.jpeg',
  },
}
export default {
  data() {
    return {
      profile: {
        name: '',
        image: '',
      },
    }
  },
  created() {
    this.fetchUser()
  },
  methods: {
    fetchUser() {
      this.profile = {
        ...dummyUser.profile,
      }
    },
    handleFileChange(e) {
      const files = e.target.files
      if (files.length === 0) {
        this.profile.image = ''
        return
      } else {
        const imageURL = window.URL.createObjectURL(files[0])
        this.profile.image = imageURL
      }
    },
    handleSubmit(e) {
      const form = e.target // <form></form>
      const formData = new FormData(form)
      this.$emit('after-submit', formData)
    },
  },
}
</script>