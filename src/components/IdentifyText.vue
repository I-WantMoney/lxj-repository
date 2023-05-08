<template>
    <div class="left">
      <div class="imageUploader">
        <input type="file" @change="identifyFromFile" accept="image/jpeg, image/png" /><br />
        <img v-bind:width="width" v-if="uploadedImage" v-bind:src="uploadedImage" />
      </div>
    </div>
    <div class="right">
      <textarea v-model="response" class="ioarea" disabled></textarea>
    </div>
  </template>
  
  <script>
  import { Predictions } from 'aws-amplify';
  import { ref } from 'vue';
  
  export default {
    setup() {
      const uploadedImage = ref(null);
      const response = ref('');
  
      const identifyFromFile = (e) => {
        const file = e.target.files || e.dataTranfer.files;
        if (!file) return;
  
        createImage(file[0]);
        // Identify Text の実装
        Predictions.identify({
          text: {
            source: {
              file: file[0],
            },
            type: 'ALL',
          },
        })
          .then((result) => {
            response.value = JSON.stringify(result, null, '\t');
          })
            .catch((error) => console.warn(error));

        // ↑↑↑↑↑↑
      };
  
      const createImage = (file) => {
        const reader = new FileReader();
        reader.onload = (e) => {
          uploadedImage.value = e.target.result;
        };
        reader.readAsDataURL(file);
      };
  
      return {
        uploadedImage,
        response,
        identifyFromFile,
      };
    },
  };
  </script>
  