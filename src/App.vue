<template>
  <v-app>
    <transition name="fade">
      <router-view/>
    </transition>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      //
    };
  },
};
</script>
<style>
  .fade-enter-active, .fade-leave-active {
  transition-property: opacity;
  transition-duration: .40s;
  }
  .fade-enter-active {
    transition-delay: .40s;
  }
  .fade-enter, .fade-leave-active {
    opacity: 0;
  }
</style>
