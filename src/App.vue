<template>
  <div id="app">
    <app-Bar></app-Bar>
    <div class="container">
      <router-view />
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  components: {
    appBar: Navbar
  }
};
</script>