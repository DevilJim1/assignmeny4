<script setup>
import moviedisplay from "./components/moviedisplay.vue";
import axios from "axios";
import {ref} from "vue";

</script>

<template>
  <div class="app-container">
    <Suspense>
      <moviedisplay />
    </Suspense>
  </div>
</template>

<style scoped>
.app-container{
  background-color:#888;
  margin-left:0px;
  margin-right:0px;
  height:1000px;
}

body {
  background-color:blachedalmond;
}
</style>

