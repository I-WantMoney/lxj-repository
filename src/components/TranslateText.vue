<template>
  <div class="left">
    <textarea class="ioarea" v-model="inputText" placeholder="入力して翻訳する。"></textarea>
  </div>
  <div class="middle">
    <textarea class="ioarea" v-model="translatedText" disabled></textarea>
  </div>
  <div class="right">
    <form>
      <input v-model="translatedText" />
      <button type = "button" @click="speech">Speech</button>
    </form>
  </div>
</template>

<script>
import { Predictions } from 'aws-amplify';
import { ref, watch } from 'vue';



export default {
  setup() {
    const inputText = ref('');
    const translatedText = ref('Translation result.');

    const translate = (text) => {
      if (!text.length) {
        translatedText.value = '';
        return;
      }

      // Text Translate の実装 ↓
      Predictions.convert({
        translateText: {
          source: {
            text: text,
          },
        },
      })
        .then((result) => {
          translatedText.value = result.text;
        })
        .catch((error) => {
          console.warn({ error });
      });
      // ↑↑↑↑↑↑
    };

    const speech = () => {
      Predictions.convert({
        textToSpeech:{
          source:{
            text: translatedText.value,
          },
        },
      })
        .then((result1)=>{
          const AudioContext = window.AudioContext || window.webkitAudioContext;
          const audioCtx = new AudioContext();
          const source = audioCtx.createBufferSource();

          audioCtx.decodeAudioData(result1.audioStream, (buffer) => {
            source.buffer = buffer;
            source.connect(audioCtx.destination);
            source.start(0);
        });
      })
      .catch((error) => console.warn(error));
    };

    watch(inputText, (inputText) => {
      translate(inputText);
    });

    return {
      inputText,
      translatedText,
      translate,
      speech,
    };
  },
};
</script>
