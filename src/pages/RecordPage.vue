<template>
  <q-page class="row q-pt-xl content-start justify-evenly">
    <q-input
      style="width: 90vw"
      outlined
      v-model="content"
      type="textarea"
    ></q-input>
    <q-input
      style="width: 90vw"
      outlined
      v-model="imgList"
      type="textarea"
    ></q-input>
    <div class="stat-bar q-mt-md">
      <span>{{ content.length }} 字数</span>|
      <q-select borderless dense v-model="device" :options="devices" />|
      <q-select borderless dense v-model="weather" :options="weathers" />
    </div>
    <q-btn
      @click="upload"
      align="right"
      dense
      class="q-pa-md q-ma-md bg-secondary text-white text-h6"
      rounded
      flat
      >发布</q-btn
    >
  </q-page>
</template>

<script setup lang="ts">
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
// @ts-ignore
import Lightgallery from 'lightgallery/vue';
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
// @ts-ignore
import lgThumbnail from 'lightgallery/plugins/thumbnail';
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
// @ts-ignore
import lgZoom from 'lightgallery/plugins/zoom';
import { reactive, ref } from 'vue';
import axios from 'axios';
const imgList = ref('');
const content = ref('');
const device = ref('Redmi Note 12 Turbo');
const devices = ref(['Redmi Note 12 Turbo', 'Mac Mini M2', 'Rog Laptop 2022']);
const weather = ref('晴');
const weathers = ref([
  '晴',
  '多云',
  '阴',
  '雨',
  '雷阵雨',
  '雪',
  '大风',
  '大雾',
  '冰雹',
  '雾霾',
  '沙尘暴',
  '日出',
  '日落',
]);

const upload = () => {
  const form = reactive({
    time: Date.now() / 1000,
    img: imgList,
    content: content,
    weather: weather,
  });
  axios.post('https://hello.stepbystep.cf/record.php', form, {
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
    },
  });
};
const plugins = ref([lgThumbnail, lgZoom]);
</script>
<style scoped lang="scss">
@import url('https://cdn.jsdelivr.net/npm/lightgallery@2.1.0-beta.1/css/lightgallery.css');
@import url('https://cdn.jsdelivr.net/npm/lightgallery@2.1.0-beta.1/css/lg-zoom.css');
@font-face {
  font-family: 'LXGW WenKai Lite';
  src: url(https://www.stepbystep.cf/assets/LXGWWenKaiLite-Regular.ttf);
}

@font-face {
  font-family: number;
  src: url(https://www.stepbystep.cf/assets/HelveticaNeueLTPro-ThEx.otf);
}

@font-face {
  font-family: Montserrat-Regular;
  src: url(https://www.stepbystep.cf/assets/Montserrat-Regular.otf);
  font-weight: 400;
  font-style: normal;
}
.stat-bar {
  display: flex;
  align-items: center;
}

.stat-bar span {
  margin-right: 8px;
  font-family: 'Montserrat-Regular';
  font-size: 1.4em;
  padding: 0 4px;
}
</style>
