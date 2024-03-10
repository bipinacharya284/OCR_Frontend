<template>
  <div class="grid justify-items-center">
    <div class="mt-2 flex justify-center rounded-lg border border-dashed border-gray-900/25 px-6 py-10">
      <div class="text-center">
        <div class="mt-4 flex justify-center text-sm leading-6 text-gray-600">
          <label
            for="file-upload"
            class="relative cursor-pointer rounded-md bg-white font-semibold text-indigo-600 focus-within:outline-none focus-within:ring-2 focus-within:ring-indigo-600 focus-within:ring-offset-2 hover:text-indigo-500"
          >
            <span>Upload a file</span>
            <input id="file-upload" name="file-upload" type="file" class="sr-only" @change="handleFileChange" />
          </label>
        </div>
        <p class="text-xs leading-5 text-gray-600">PNG, JPG, GIF up to 10MB</p>
      </div>
    </div>
    <div class="mt-4">
      <button
        @click="uploadFile"
        class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring focus:border-blue-300"
      >
        Predict
      </button>
    </div>

    <div v-if="serverResponse" class="mt-4 grid justify-items-center">
      <textarea readonly class="w-full px-3 py-2 text-gray-700 border rounded-lg focus:outline-none" rows="4" v-model="serverResponse">..</textarea>
      <button @click="copyToClipboard" class="mt-2 px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600 focus:outline-none focus:ring focus:border-green-300">
        Copy to Clipboard
      </button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      file: null,
      serverResponse:null,
    };
  },
  methods: {
    handleFileChange(event) {
      // Updating the 'file' property when a file is selected
      this.file = event.target.files[0];
    },
    uploadFile() {
      // Basic validation
      if (!this.file) {
        alert('Please select a file');
        return;
      }

      // Preparing form data for POST request
      const formData = new FormData();
      formData.append('file', this.file);

      axios
        .post('http://127.0.0.1:8000/predict/', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        .then((response) => {
          // Handling the successful response
          console.log('File uploaded successfully:', response.data);

          // Storing the server response
          this.serverResponse = response.data.predictions;

          // Clearing the file input after upload
          this.file = null;
        })
        .catch((error) => {
          // Handldling error
          console.error('Error uploading file:', error);
        });
    },
    copyToClipboard() {
      navigator.clipboard.writeText(this.serverResponse).then(() => {
        alert('Text copied to clipboard');
      }).catch(err => {
        console.error('Could not copy text: ', err);
      });
    }
  },
};
</script>

<style scoped>

</style>