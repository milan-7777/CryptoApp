<script setup>
import TableCoins from '@/components/TableCoins.vue';
import Loader from '@/components/Loader.vue'
import { ref } from 'vue'
let isLoaded = ref (false) 

function closeLoader() {
  isLoaded.value = true
  console.log('')
}
</script>

<template>
  <div class="cryptoApp">
    <TableCoins @finish="closeLoader" v-show="isLoaded"/>
    <Loader v-show="!isLoaded"/>

  </div>
</template>

<style>
body {
  background-image: url('https://images.pexels.com/photos/325185/pexels-photo-325185.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1');
  background-size: cover;
  background-repeat: no-repeat;
  z-index: -2;
  font-family: sans-serif;
  letter-spacing: 2px;
}
</style>
