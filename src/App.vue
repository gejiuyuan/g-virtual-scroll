<script setup lang="ts"> 
 
import { ref } from "vue";
import List from "./components/list.vue";

const ff = ref('ff');
</script>

<template>
  <h1>哈哈哈</h1>
  <List msg="ff"></List>
 
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
