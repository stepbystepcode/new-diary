<template>
  <q-page class="row q-pt-xl items-start justify-evenly">
    <q-card
      v-for="(article, index) in lists"
      :key="index"
      class="q-ma-md q-pa-lg card"
    >
      <div class="row items-baseline">
        <div class="list-day-text color-text">
          {{ moments(index, 'D') }}
        </div>
        <div class="list-time-text q-ml-md col-grow color-desc">
          {{ moments(index, 'M') }}月 · {{ moments(index, 'T') }}
        </div>
      </div>
      <div
        class="list-content-text q-mt-md color-text"
        style="
          word-break: break-all;
          overflow-wrap: break-word;
          font-size: 1.4em;
        "
        v-html="article.content"
      ></div>
      <div class="row items-center q-mt-md justify-start">
        <div v-if="imgList[index]">
          <lightgallery
            :settings="{ speed: 500, plugins: plugins }"
          >
            <a
              v-for="(img, i) in imgList[index]"
              :key="i"
              :data-src="`https://img.gejiba.com/images/${img}`"
            >
              <img
                class="list-image q-mr-sm"
                :src="`https://img.gejiba.com/images/${img}`"
              />
            </a>
          </lightgallery>
        </div>
      </div>
      <div class="row items-center q-mt-md no-wrap">
        <div class="color-desc" style="font-size: 1.4em">
          {{ article.content.length - 7 }} 字 · Redmi Note 12 Turbo ·
          {{ article.weather }}
        </div>
        <div class="q-space"></div>
      </div>
    </q-card>
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
import { ref } from 'vue';
import axios from 'axios';
interface Article {
  content: string;
  date: number;
  img: string;
  weather: string;
}
const lists = ref<Article[]>([]);
const imgList = ref<string[][]>([]);
const plugins = ref([lgThumbnail, lgZoom]);

axios.get('https://hello.stepbystep.cf/fetch.php').then((res) => {
  lists.value = res.data;
  for (let i = 0; i < lists.value.length; i++) {
    let array = lists.value[i].img.split(' ');
    imgList.value.push(array);
  }
});

const moments = (index:number, type:string) => {
  let time = new Date(lists.value[index].date * 1000);
  switch (type) {
    case 'Y':
      return time.getFullYear();
    case 'M':
      return time.getMonth();
    case 'D':
      return time.getDate();
    case 'T':
      return `${time.getHours()}:${(time.getMinutes() + '').padStart(2, '0')}`;
  }
};
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

.color-text {
  color: #333;
}

.list-image {
  width: 153px;
  height: 153px;
  object-fit: cover;
  border-radius: 8px;
  background-color: #eee;
}

@media screen and (max-width: 700px) {
  .list-image {
    width: 86px;
    height: 86px;
  }
}

.list-day-text {
  font-size: 26px;
  font-family: Montserrat-Regular;
}

.list-time-text {
  font-size: 14px;
  font-family: Montserrat-Regular;
}

.list-content-text {
  font-size: 1em;
  line-height: 180%;
  min-height: 50px;
}

.color-desc {
  color: #a2a2a2;
  font-size: 1.2em;
}

.body--light .q-page {
  transition: background-color .5s;
  background-color: #fafafa;
}
.body--dark .q-page {
  transition: background-color .5s;
  background-color: #121212;
}
.card {
  min-width: 90vw;
  border-radius: 8px;
  box-shadow: 0 2px 4px 2px rgb(0 0 0/7%);
  transition: background-color .5s;
}

</style>
