<template>
  <div class="left">
    <textarea
      class="ioarea"
      v-model="inputText"
      placeholder="Type to interpret.（Press Shift + Enter）"
      @keydown.enter.shift="interpretation"
    ></textarea>
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
    const inputText = ref('');
    const response = ref('');

    const interpretation = (e) => {
      e.preventDefault();
      // Interpret Textの実装
      Predictions.interpret({
        text: {
          source: {
            text: inputText.value,
          },
        },
      })
        .then((result) => {
          response.value = JSON.stringify(result, null, '\t');
        })
        .catch((error) => console.warn(error));
      // ↑↑↑↑↑↑
    };

    return {
      inputText,
      response,
      interpretation,
    };
  },
};
</script>
