<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="header" flat :style="{background:headerBgColor}">
      <q-toolbar>
        <q-btn round flat @click="goToSite"><q-icon name="img:/home.svg" /></q-btn>
        <q-toolbar-title class="text-center" style="font-size:1.1em"> Step by step の Diary </q-toolbar-title>
        <q-btn @click="toggleTheme" round flat><q-icon :name="`img:/${mode}.svg`" /></q-btn>
      </q-toolbar>
    </q-header>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { useQuasar } from 'quasar'
import { ref } from 'vue';
const $q = useQuasar()
const headerBgColor= ref('#fff');
const mode= ref('moon');
const goToSite = ()=> {
      window.location.href = 'https://www.stepbystep.cf/';
    }
    const toggleTheme=()=> {
      $q.dark.toggle();
      headerBgColor.value=$q.dark.isActive ? '#292828':'#fff';
      mode.value=$q.dark.isActive ? 'sun':'moon';
    }
</script>
<style scoped lang="scss">

header {
  height: 5rem;
  line-height: 5rem;
  box-shadow: 0px 1px 8px 1px rgb(0 0 0 / 7%);
  font-size: 18px;
  z-index: 1000;
  color: #333;
}
</style>
