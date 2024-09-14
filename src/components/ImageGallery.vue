<template>
  <div class="gallery">
    <input type="file" id="fileInput" @change="uploadImage" accept="image/*" />
    <label for="fileInput" class="upload-btn">Upload Image</label>
    <div class="gallery-grid">
      <ImageItem
        v-for="(image, index) in images"
        :key="image.id"
        :image="image"
        @click="openModal(image, index)"
      />
    </div>
    <ImageModal
      v-if="isModalOpen"
      :image="activeImage"
      :images="images"
      :currentIndex="currentImageIndex"
      @close="closeModal"
      @change-image="changeImage"
      @delete="deleteImage"
    />
  </div>
</template>

<script>
import ImageItem from './ImageItem.vue';
import ImageModal from './ImageModal.vue';

export default {
  components: { ImageItem, ImageModal },
  data() {
    return {
      images: [
        { id: 1, title: 'Sunset', url: require('@/assets/sunset.png'), description: 'A beautiful sunset', dateUploaded: '2024-08-01' },
        { id: 2, title: 'Mountains', url: require('@/assets/mountains.png'), description: 'Majestic mountains', dateUploaded: '2024-08-05' },
        { id: 3, title: 'Moon', url: require('@/assets/moon.png'), description: 'A serene moon', dateUploaded: '2024-08-12' }
      ],
      isModalOpen: false,
      activeImage: null,
      currentImageIndex: 0
    };
  },
  methods: {
    uploadImage(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          const newImage = {
            id: Date.now(),
            url: e.target.result,
            title: file.name,
            description: prompt('Enter a description for the image:') || 'No description',
            dateUploaded: new Date().toISOString().split('T')[0] 
          };
          this.images.push(newImage);
        };
        reader.readAsDataURL(file);
      }
    },
    openModal(image, index) {
      this.activeImage = image;
      this.currentImageIndex = index;
      this.isModalOpen = true;
    },
    closeModal() {
      this.isModalOpen = false;
      this.activeImage = null;
      this.currentImageIndex = 0;
    },
    changeImage(index) {
      this.activeImage = this.images[index];
      this.currentImageIndex = index;
    },
    deleteImage(index) {
      if (confirm('Are you sure you want to delete this image?')) {
        this.images.splice(index, 1);
        if (this.currentImageIndex === index) {
          this.closeModal();
        } else if (this.currentImageIndex > index) {
          this.currentImageIndex--;
        }
      }
    }
  }
};
</script>

<style scoped>
.gallery {
  padding: 2rem;
  text-align: center;
}

#fileInput {
  display: none; 
}

.upload-btn {
  display: inline-block;
  padding: 0.75rem 1.5rem;
  background-color: #6db8cf; 
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  margin-bottom: 1rem;
  transition: background-color 0.3s, transform 0.2s; 
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}

.gallery-grid img {
  width: 100%;
  height: auto;
  border-radius: 4px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s;
}

.gallery-grid img:hover {
  transform: scale(1.05); 
}
</style>
