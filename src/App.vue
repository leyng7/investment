<template>
  <v-container
      fluid
      style="max-width: 500px;"
  >
      <HelloWorld/>
  </v-container>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>

#app {
  touch-action: pan-x pan-y;
}

.v-field__input {
  text-align: right;
  margin-right: 5px;
}
</style>
