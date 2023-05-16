<template>
  <input id="check1" class="check" type="radio" name="ai" />
  <label class="label" for="check1"> Translate & Speech - Amazon Translate & Speech </label>
  <div class="content">
    <TranslateText />
  </div>

  <!-- <input id="check2" class="check" type="radio" name="ai" />
  <label class="label" for="check2"> Indentify Labels - Amazon Rekognition </label>
  <div class="content">
    <IdentifyLabels />
  </div>

  <input id="check3" class="check" type="radio" name="ai" />
  <label class="label" for="check3"> Indentify Text - Amazon Rekognition </label>
  <div class="content">
    <IdentifyText />
  </div> -->

  <input id="check2" class="check" type="radio" name="ai" />
  <label class="label" for="check2"> Interpret Text - Amazon Comprehend </label>
  <div class="content">
    <InterpretText />
  </div>

  <!-- <input id="check3" class="check" type="radio" name="ai" />
  <label class="label" for="check3"> Text to Speech - Amazon Polly </label>
  <div class="content">
    <TextSpeech />
  </div> -->
</template>

<script>
import TranslateText from '@/components/TranslateText.vue';
// import IdentifyLabels from '@/components/IdentifyLabels.vue';
// import IdentifyText from '@/components/IdentifyText.vue';
import InterpretText from '@/components/InterpretText.vue';
// 追加 2
import TextSpeech from '@/components/TextSpeech.vue';

export default {
  components: {
    TranslateText,
    // IdentifyLabels,
    // IdentifyText,
    InterpretText,
    // 追加 3
    TextSpeech,
  },
};
</script>

