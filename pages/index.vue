<template>
  <div>
    <v-file-input v-model="file" label="Upload Image"></v-file-input>
    <button @click="uploadImage">Submit</button>
    <img v-if="imageUrl" :src="imageUrl" />
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      file: null,
      imageUrl: null
    }
  },
  methods: {
    uploadImage () {
      const formData = new FormData()
      formData.append('image', this.file)
      axios.post('http://localhost:4000/upload', formData)
        .then((response) => {
          this.imageUrl = response.data.imageUrl
        })
        .catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>
