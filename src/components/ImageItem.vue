<template>
  <div class="image-item" @click="$emit('click')">
    <img :src="image.url" :alt="image.title" />
  </div>
</template>

<script>
export default {
  props: {
    image: Object
  }
};
</script>

<style scoped>
.image-item {
  position: relative;
  cursor: pointer;
}

.image-item img {
  width: 100%;
  height: auto;
  border-radius: 4px;
}

</style>
