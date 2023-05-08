<template>
  <div class="left">
    <div class="imageUploader">
      <div class="file">
        <input type="file" @change="identifyFromFile" accept="image/jpeg, image/png" />
      </div>
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

      //Identify Labels の実装
      Predictions.identify({
        labels: {
          source: {
            file: file[0],
          },
          type: 'LABELS',
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

