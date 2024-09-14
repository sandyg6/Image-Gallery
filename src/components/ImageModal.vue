<template>
  <div class="modal" @click.self="closeModal">
    <div class="modal-content">
      <img :src="image.url" :alt="image.title" :style="{ transform: `scale(${scale})` }" />
      <button 
        v-if="!isFirstImage"
        class="prev"
        @click="prevImage">
        Previous
      </button>
      <button 
        v-if="!isLastImage"
        class="next"
        @click="nextImage">
        Next
      </button>
      <div class="bottom-controls">
        <button class="zoom-out" @click="zoomOut">Zoom Out</button>
        <button class="zoom-in" @click="zoomIn">Zoom In</button>
        <button class="delete" @click="deleteImage">Delete</button>
        <button class="metadata-toggle" @click="toggleMetadata">Metadata</button>
      </div>
      <button class="close" @click="closeModal">Close</button>
      <div v-if="showMetadata" class="metadata-info">
        <p class="title">{{ image.title }}</p>
        <p class="description">{{ image.description }}</p>
        <p class="date">{{ image.dateUploaded }}</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    image: Object,
    images: Array,
    currentIndex: Number
  },
  data() {
    return {
      scale: 1,
      showMetadata: false
    };
  },
  computed: {
    isFirstImage() {
      return this.currentIndex === 0;
    },
    isLastImage() {
      return this.currentIndex === this.images.length - 1;
    }
  },
  methods: {
    closeModal() {
      this.$emit('close');
    },
    prevImage() {
      if (this.isFirstImage) return;
      const prevIndex = this.currentIndex - 1;
      this.$emit('change-image', prevIndex);
    },
    nextImage() {
      if (this.isLastImage) return;
      const nextIndex = this.currentIndex + 1;
      this.$emit('change-image', nextIndex);
    },
    zoomIn() {
      this.scale = Math.min(this.scale + 0.1, 3);
    },
    zoomOut() {
      this.scale = Math.max(this.scale - 0.1, 1); 
    },
    deleteImage() {
      if (confirm('Are you sure you want to delete this image?')) {
        this.$emit('delete', this.currentIndex);
      }
    },
    toggleMetadata() {
      this.showMetadata = !this.showMetadata;
    }
  }
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  position: relative;
  max-width: 90vw; 
  max-height: 90vh; 
  display: flex;
  flex-direction: column;
  align-items: center;
}
img {
  max-width: 100%;
  max-height: 100%;
}
button {
  position: absolute;
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  z-index: 10;
}
.close {
  top: 1rem;
  right: 1rem;
}
.prev {
  left: 1rem;
  top: 50%;
  transform: translateY(-50%);
}
.next {
  right: 1rem;
  top: 50%;
  transform: translateY(-50%);
}
.bottom-controls {
  position: absolute;
  bottom: 1rem;
  display: flex;
  justify-content: center;
  gap: 1rem;
  width: 100%;
}
.zoom-in,
.zoom-out {
  background-color: rgba(0, 0, 0, 0.7);
  padding: 0.5rem 1rem;
}
.zoom-out {
  margin-left: 10%;
}
.zoom-in {
  margin-right: 10%;
}
.delete {
  position: absolute;
  bottom: 0;
  right: 1rem;
  background-color: rgba(255, 0, 0, 0.7); 
}
.metadata-toggle {
  position: absolute;
  bottom: 1rem;
  left: 1rem;
}
.metadata-info {
  position: absolute;
  bottom: 4rem;
  left: 0;
  right: 0;
  padding: 4px;
  background: antiquewhite;
  color: #000000;
  text-align: center;
  border-radius: 4px;
  width: 20%;
}
.title {
  font-weight: bold;
}
.description {
  margin: 0.5rem 0;
}
.date {
  font-size: 0.8rem;
}
button:disabled {
  cursor: not-allowed;
  background-color: rgba(0, 0, 0, 0.5); 
}
</style>
