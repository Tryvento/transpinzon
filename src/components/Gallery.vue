<template>
  <section id="gallery" class="gallery-section" @click="nextImage">
    <div class="gallery-container">
      <div class="gallery-stack">
        <div
          v-for="(image, index) in images"
          :key="image.id"
          class="gallery-item"
          :class="{
            active: currentIndex === index,
            next: (currentIndex + 1) % images.length === index,
            prev: (currentIndex - 1 + images.length) % images.length === index,
          }"
          :style="{ 'z-index': currentIndex === index ? 10 : images.length - index }"
        >
          <h3>{{ image.title }}</h3>
          <img :src="image.image" :alt="image.title" :style="{ transform: `rotate(${image.rotation}deg)` }"/>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import service1 from '@/assets/img/placeholders/service1.png'
import service2 from '@/assets/img/placeholders/service2.png'
import service3 from '@/assets/img/placeholders/service3.png'
import service4 from '@/assets/img/placeholders/service4.png'

const currentIndex = ref(0)

const images = [
  {
    id: 1,
    image: service1,
    title: 'Imagen 1',
    rotation: 4,
  },
  {
    id: 2,
    image: service2,
    title: 'Imagen 2',
    rotation: -2,
  },
  {
    id: 3,
    image: service3,
    title: 'Imagen 3',
    rotation: 3,
  },
  {
    id: 4,
    image: service4,
    title: 'Imagen 4',
    rotation: -1,
  },
  {
    id: 5,
    image: service1,
    title: 'Imagen 5',
    rotation: 2,
  },
  {
    id: 6,
    image: service2,
    title: 'Imagen 6',
    rotation: -3,
  },
]

const nextImage = () => {
  currentIndex.value = (currentIndex.value + 1) % images.length
}

</script>

<style scoped>
.gallery-section {
  width: 100%;
  height: 00px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  perspective: 1000px;
  cursor: pointer;
}

.gallery-container {
  position: relative;
  width: 250px;
  height: 250px;
  margin: 0 auto;
}


.gallery-stack {
  position: relative;
  width: 100%;
  height: 100%;
}

.gallery-item {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  border-radius: 10px;
  transition: all 0.5s ease;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.gallery-item h3 {
  position: absolute;
  top: 10px;
  left: 10px;
  color: var(--white);
  font-size: 1.4rem;
  font-weight: bold;
  font-family: var(--font-title);
  filter: drop-shadow(0px 0px 10px rgba(0, 0, 0, 0.5));
}

.gallery-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 10px;
  transition: transform 0.5s ease;
}

/* Estado activo - imagen actual */
.gallery-item.active {
  opacity: 1;
  transform: translateX(0);
  z-index: 10;
}

/* Próxima imagen - preparada para entrar */
.gallery-item.next {
  opacity: 0;
  transform: translateX(30px);
  z-index: 5;
}

/* Imagen anterior - saliendo */
.gallery-item.prev {
  opacity: 0;
  transform: translateX(-30px);
  z-index: 1;
}

/* Efecto hover sutil */
.gallery-item.active:hover img {
  transform: scale(1.02);
}

@media (min-width: 768px) {
  .gallery-container {
    width: 400px;
    height: 400px;
  }
  .gallery-item h3{
    font-size: 1.7rem;
    font-weight: bolder;
    z-index: 10;
  }
}

@media (min-width: 1200px) {
  .gallery-container {
    width: 600px;
    height: 600px;
  }
}
</style>
