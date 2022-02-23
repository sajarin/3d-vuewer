
<template>
  <div class="container"> 
    <h1>Upload images</h1>
    <div class="dropbox">
      <input type="file" accept=".dae" v-on:drop="loadText" @change="loadText">
      <p id="input-desc" ref="desc"> Drag your .dae file here to begin<br> or click to browse </p>
    </div>
  </div>
</template>

<script>
  export default {
    methods: {
      loadText(e) {
        const file = e.target.files[0]
        const reader = new FileReader()
        reader.onload = e => {
          const text = e.target.result
          this.$emit('load', text); 
        }
        reader.readAsText(file)
      },
    }
  }

</script>
<style scoped>
  .container{
    display: flex;
    flex-direction: column;
    width: 65%;
    margin: 0 auto;
  }
  .dropbox {
    display: flex;
    justify-content: center;
    align-content: center;
    flex-direction: column;
    outline: 2px dashed black;
    outline-offset: -10px;
    background: #C0D5E9;
    color: dimgray;
    padding: 10px 10px;
    margin-bottom: 2em;
    min-height: 65vh;
    position: relative;
    cursor: pointer;
  }
  .dropbox:hover {
    background: #6A8793;
  }
  input {
    opacity: 0;
    width: 100%;
    height: 100%;
    position: absolute;
    cursor: pointer;
  }
  #input-desc {
    text-align: center;
    color: black;
  }
</style>