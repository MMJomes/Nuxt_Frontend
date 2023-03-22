<template>
  <v-app>
    <v-container>
      <v-app-bar app color="light-blue" white>
        <v-toolbar-title>My App</v-toolbar-title>
        <v-spacer />
        <v-btn>Log In</v-btn>
      </v-app-bar>
      <v-main>
        <v-container>
          <div>
            <v-file-input v-model="file" label="Upload Image" />
            <button @click="uploadImage">Submit</button>
            <img v-if="imageUrl" :src="imageUrl" />
          </div>
        </v-container>
      </v-main>
    </v-container>
  </v-app>
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
