<script setup>
import { ref } from 'vue'
import WheelNav from './WheelNav.vue'
import Who from './Who.vue'
import Services from './Services.vue'
import Gallery from './Gallery.vue'

const sections = [
  {
    id: 'who',
    title: 'Quiénes Somos',
    content:
      'En Transpinzon FJ SAS, nos dedicamos a brindar servicios de transporte logístico confiable, rápido y seguro en todo el territorio nacional.',
    icon: 'fa-solid:users',
  },
  {
    id: 'services',
    title: 'Servicios',
    content:
      'Ofrecemos una amplia gama de servicios de transporte logístico, desde la carga y descarga de mercancías hasta la entrega final.',
    icon: 'proicons:box',
  },
  {
    id: 'gallery',
    title: 'Galería',
    content: 'Si tienes alguna pregunta o necesitas más información, no dudes en contactarnos.',
    icon: 'streamline-flex:gallery',
  },
]
const currentId = ref('who')

const changeSection = (id) => {
  if (id !== currentId.value) {
    currentId.value = id
  }
}
</script>

<template>
  <section class="about-container" id="about">
    <div class="wheel-side">
      <WheelNav :active-id="currentId" @change="changeSection" :sections="sections" />
    </div>
    <transition name="fade-slide" mode="out-in">
      <div :key="currentId" class="content-side">
        <div v-if="currentId === 'who'" class="content">
          <Who />
        </div>
        <div v-if="currentId === 'services'" class="content">
          <Services />
        </div>
        <div v-if="currentId === 'gallery'" class="content">
          <Gallery />
        </div>
      </div>
    </transition>
  </section>
</template>

<style scoped>
.about-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2rem;
  width: 100dvw;
  flex-direction: column;
}

.wheel-side {
  width: 100dvw;
  display: flex;
  justify-content: center;
  order: 2;
}

.content-side {
  width: 100%;
  height: auto;
  transition: height .3s ease;
  max-width: 800px;
  min-height: 500px;
  max-height: 700px;
  order: 1;
  margin-bottom: 2rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

@media (min-width: 1024px) {
  .about-container {
    flex-direction: row;
    height: 100vh;
  }

  .wheel-side {
    order: 1;
    flex: 1;
  }

  .content-side {
    order: 2;
    flex: 2;
    margin-bottom: 0;
  }
  .content {
    transform: translateX(-3rem);
  }
}

.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.5s ease;
}
.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(15px);
}
.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(-15px);
}

</style>
