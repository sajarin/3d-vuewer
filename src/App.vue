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
    <Upload v-if="showUpload === true" @load="onLoad"/>
    <Scene v-if="showUpload === false" :contents="file"  v-on:btn-click="handleDerender"/>
  </main>
</template>

<script>
  export default {
    data() {
      return {
        file: null,
        showUpload: true
      }
    },
    methods: {
      onLoad: function(fileContents) {
        this.file = fileContents
        this.showUpload = false
      },  
      handleDerender: function() {
        this.file = null
        this.showUpload = true 
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