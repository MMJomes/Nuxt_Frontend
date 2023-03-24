<template>
  <v-app>
    <v-container>
      <v-app-bar app color="light-blue" white>
        <v-toolbar-title>Sample Image Upload Code Test Application</v-toolbar-title>
        <v-spacer />
        <v-btn  @click="gotoImageList">
        All Image Lists
      </v-btn>
      </v-app-bar>
      <v-main>
        <v-container>
          <div>
            <v-file-input v-model="file" label="Upload Image" />
            <button @click="uploadImage">
              Submit
            </button>
            <img v-if="imageUrl" :src="imageUrl">
          </div>
          <v-alert v-if="showSuccess" type="success">Image uploaded successfully!</v-alert>
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
      imageUrl: null,
      showSuccess: false,
      showError: false
    }
  },
  methods: {
    gotoImageList () {
      this.$router.push('/about')
    },
    uploadImage () {
      if (!this.file) {
        alert('Please select an image')
        return
      } else {
        this.error = ''
      }
      if (!this.file.type.includes('image')) {
        this.showSuccess = false
        this.showError = true
        alert('Please select an image')
        this.file = null
        return
      }
      const formData = new FormData()
      formData.append('image', this.file)
      axios.post('http://localhost:4000/upload', formData)
        .then((response) => {
          this.imageUrl = response.data.imageUrl
          if (response.status === 200) {
            this.snackbar = true
            this.showSuccess = true
            this.message = 'Your image has been uploaded successfully'
            setTimeout(() => {
              this.showSuccess = false
            }, 1000)
            setTimeout(() => {
              this.$router.push('/about')
            }, 1000)
            this.file = null
          }
          console.log(response.data)
        })
        .catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>
