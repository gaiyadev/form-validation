<template>
  <div class="home">
    <app-Home></app-Home>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    appHome: HelloWorld
  }
};
</script>
