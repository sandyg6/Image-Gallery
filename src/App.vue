<template>
  <div id="app">
    <header>
      <h1>Image Gallery</h1>
    </header>
    <main>
      <ImageGallery />
    </main>
  </div>
</template>

<script>

import ImageGallery from './components/ImageGallery.vue';

export default {
  components: {
    ImageGallery
  }
};
</script>

<style>
*{
  background: antiquewhite;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

header {

  color: rgb(15, 116, 163);
  padding: 1rem;
}

header h1 {
  margin: 0;
}

main {
  padding: 1rem;
}
</style>
