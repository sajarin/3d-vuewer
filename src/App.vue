<script setup>
  import Header from './components/Header.vue';
  import Upload from './components/Upload.vue';
  import Scene from './components/Scene.vue';
</script>

<template>
  <header>
      <Header color="black" text="3D VUEWER"/>
  </header>
  <main>
    <Upload v-if="file === null" @load="onLoad"/>
    <Scene v-if="file != null" :contents="file" v-on:btn-click="handleRender"/>
  </main>
</template>

<script>
  export default {
    data() {
      return {
        file: null
      }
    },
    methods: {
      onLoad: function(fileContents) {
        this.file = fileContents
      },  
      handleRender: function() {
        this.file = null
      }
    }
  }
</script>

<style>
@font-face {
  font-family: "Garet Heavy";
  src: local("Garet Heavy"), url(./assets/Garet.otf) format("opentype")
}
#app {
  margin-left: 10%;
  margin-right: 10%;
  margin-top: 2%;
}
</style>