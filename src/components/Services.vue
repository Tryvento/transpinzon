<template>
  <section id="services" class="services-section">
    <div class="services-container">
      <div class="section-header">
        <h2>Nuestros Servicios</h2>
        <p>Soluciones integrales para todas sus necesidades logísticas</p>
      </div>

      <div class="slider-wrapper">
        <button 
          class="slider-arrow left-arrow" 
          @click="prevSlide" 
          :disabled="currentIndex === 0"
          aria-label="Anterior"
        >
          <Icon icon="picon:left" class="icon" />
        </button>

        <div class="slider-viewport">
          <div
            class="slider-track"
            :style="{ transform: `translateX(-${currentIndex * (100 / itemsPerView)}%)` }"
          >
            <div
              v-for="(service, index) in services"
              :key="service.id"
              class="service-card-wrapper"
              :style="{ width: `${100 / itemsPerView}%` }"
            >
              <div class="service-card" @click="viewServiceDetails(service)">
                <div class="card-image">
                  <img :src="service.image" :alt="service.title" loading="lazy" />
                  <div class="card-overlay">
                    <span class="view-more-text">Ver más</span>
                  </div>
                </div>
                <div class="card-content">
                  <h3>{{ service.title }}</h3>
                  <p>{{ service.description }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <button 
          class="slider-arrow right-arrow" 
          @click="nextSlide" 
          :disabled="currentIndex >= maxIndex"
          aria-label="Siguiente"
        >
          <Icon icon="picon:right" class="icon" />
        </button>
      </div>

      <!-- Dots Navigation -->
      <div class="slider-dots">
        <button
          v-for="(_, index) in Math.max(0, services.length - itemsPerView + 1)"
          :key="index"
          :class="{ active: currentIndex === index }"
          @click="goToSlide(index)"
          :aria-label="`Ir al grupo ${index + 1}`"
        ></button>
      </div>
    </div>

    <!-- Modal de Detalles -->
    <Transition name="modal-fade">
      <div v-if="selectedService" class="modal-overlay" @click.self="closeDetails">
        <div class="modal-content">
          <button class="btn-close" @click="closeDetails" aria-label="Cerrar">
            <Icon icon="picon:close" />
          </button>
          
          <div class="modal-body">
            <div class="modal-image">
              <img :src="selectedService.image" :alt="selectedService.title" />
            </div>
            <div class="modal-info">
              <h3>{{ selectedService.title }}</h3>
              <p class="modal-desc">{{ selectedService.description }}</p>
              
              <button class="btn-contact" @click="contactService">
                Solicitar Cotización
                <Icon icon="picon:mail" class="icon" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed } from 'vue'

// Importar imágenes
import service1 from '@/assets/img/placeholders/service1.png'
import service2 from '@/assets/img/placeholders/service2.png'
import service3 from '@/assets/img/placeholders/service3.png'
import service4 from '@/assets/img/placeholders/service4.png'

const services = [
  {
    id: 1,
    title: 'Transporte de Carga',
    description: 'Soluciones logísticas integrales para el transporte seguro de su mercancía a nivel nacional.',
    image: service1,
  },
  {
    id: 2,
    title: 'Distribución Local',
    description: 'Servicio de distribución eficiente en el área metropolitana con seguimiento en tiempo real.',
    image: service2,
  },
  {
    id: 3,
    title: 'Almacenamiento',
    description: 'Espacios seguros y climatizados para el almacenamiento de su mercancía.',
    image: service3,
  },
  {
    id: 4,
    title: 'Logística Personalizada',
    description: 'Soluciones a medida para las necesidades específicas de su negocio.',
    image: service4,
  },
]

const currentIndex = ref(0)
const selectedService = ref(null)
const itemsPerView = ref(1)
let autoSlideInterval

// Calcular items visibles según el ancho de pantalla
const updateItemsPerView = () => {
  if (window.innerWidth >= 1024) {
    itemsPerView.value = 3
  } else if (window.innerWidth >= 768) {
    itemsPerView.value = 2
  } else {
    itemsPerView.value = 1
  }
  // Ajustar índice si excede el nuevo máximo
  if (currentIndex.value > maxIndex.value) {
    currentIndex.value = maxIndex.value
  }
}

const maxIndex = computed(() => Math.max(0, services.length - itemsPerView.value))

const nextSlide = () => {
  if (currentIndex.value < maxIndex.value) {
    currentIndex.value++
  } else {
    currentIndex.value = 0 // Loop back to start
  }
}

const prevSlide = () => {
  if (currentIndex.value > 0) {
    currentIndex.value--
  } else {
    currentIndex.value = maxIndex.value // Loop to end
  }
}

const goToSlide = (index) => {
  currentIndex.value = index
}

const viewServiceDetails = (service) => {
  selectedService.value = service
  stopAutoSlide()
  document.body.style.overflow = 'hidden' // Prevent background scrolling
}

const closeDetails = () => {
  selectedService.value = null
  startAutoSlide()
  document.body.style.overflow = ''
}

const contactService = () => {
  console.log('Contactar por:', selectedService.value.title)
  // Implement navigation to contact section or open contact modal
}

const startAutoSlide = () => {
  stopAutoSlide()
  autoSlideInterval = setInterval(() => {
    nextSlide()
  }, 6000)
}

const stopAutoSlide = () => {
  if (autoSlideInterval) clearInterval(autoSlideInterval)
}

onMounted(() => {
  updateItemsPerView()
  window.addEventListener('resize', updateItemsPerView)
  startAutoSlide()
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', updateItemsPerView)
  stopAutoSlide()
})
</script>

<style scoped>
.services-section {
  padding: var(--space-xl) 0;
  width: 100%;
  background-color: var(--background); /* Or a slightly different shade if needed */
  overflow: hidden;
}

.services-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 var(--space-md);
  width: 100%;
}

.section-header {
  text-align: center;
  margin-bottom: var(--space-xl);
}

.section-header h2 {
  font-family: var(--font-title);
  font-size: 2.5rem;
  color: var(--primary);
  margin-bottom: var(--space-xs);
}

.section-header p {
  color: var(--text-muted);
  font-size: 1.1rem;
}

/* Slider Styles */
.slider-wrapper {
  position: relative;
  width: 100%;
  padding: 0 var(--space-lg); /* Space for arrows */
}

.slider-viewport {
  overflow: hidden;
  width: 100%;
  border-radius: var(--radius-lg);
}

.slider-track {
  display: flex;
  transition: transform 0.5s cubic-bezier(0.25, 1, 0.5, 1);
  width: 100%;
}

.service-card-wrapper {
  padding: 0 var(--space-sm);
  flex-shrink: 0;
}

.service-card {
  background: var(--white);
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow);
  transition: var(--transition);
  height: 100%;
  display: flex;
  flex-direction: column;
  border: 1px solid rgba(0,0,0,0.05);
  cursor: pointer; /* Make it obvious it's clickable */
}

.service-card:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-hover);
}

.card-image {
  height: 220px;
  position: relative;
  overflow: hidden;
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s ease;
}

.service-card:hover .card-image img {
  transform: scale(1.1);
}

.card-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: var(--transition);
}

.service-card:hover .card-overlay {
  opacity: 1;
}

.view-more-text {
  color: var(--white);
  font-weight: 600;
  font-size: 1.1rem;
  border: 2px solid var(--white);
  padding: 0.5rem 1rem;
  border-radius: var(--radius);
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(4px);
}

.card-content {
  padding: var(--space-md);
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.card-content h3 {
  font-family: var(--font-title);
  color: var(--primary);
  font-size: 1.4rem;
  margin-bottom: var(--space-sm);
}

.card-content p {
  color: var(--text-light);
  font-size: 0.95rem;
  line-height: 1.6;
  margin-bottom: var(--space-md);
  flex-grow: 1;
}

/* Navigation Arrows */
.slider-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: var(--white);
  border: 1px solid var(--border);
  color: var(--primary);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 10;
  box-shadow: var(--shadow);
  transition: var(--transition);
}

.slider-arrow:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.slider-arrow:not(:disabled):hover {
  background: var(--primary);
  color: var(--white);
  transform: translateY(-50%) scale(1.1);
}

.left-arrow { left: 0; }
.right-arrow { right: 0; }

.slider-dots {
  display: flex;
  justify-content: center;
  margin-top: var(--space-lg);
  gap: 8px;
}

.slider-dots button {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--border);
  border: none;
  cursor: pointer;
  transition: var(--transition);
}

.slider-dots button.active {
  background: var(--primary);
  transform: scale(1.2);
}

/* Modal Styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(5px);
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-md);
}

.modal-content {
  background: var(--white);
  border-radius: var(--radius-lg);
  width: 100%;
  max-width: 900px;
  max-height: 90vh;
  overflow-y: auto;
  position: relative;
  box-shadow: var(--shadow-hover);
  display: flex;
  flex-direction: column;
}

.btn-close {
  position: absolute;
  top: var(--space-md);
  right: var(--space-md);
  background: rgba(255, 255, 255, 0.8);
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
  transition: var(--transition);
  color: var(--text);
}

.btn-close:hover {
  background: var(--secondary);
  color: var(--white);
}

.modal-body {
  display: flex;
  flex-direction: column;
}

.modal-image {
  width: 100%;
  height: 300px;
}

.modal-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.modal-info {
  padding: var(--space-xl);
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.modal-info h3 {
  font-family: var(--font-title);
  font-size: 2rem;
  color: var(--primary);
  margin-bottom: var(--space-sm);
}

.modal-desc {
  font-size: 1.1rem;
  color: var(--text-light);
  margin-bottom: var(--space-lg);
  line-height: 1.7;
}

.btn-contact {
  background: var(--gradient-primary);
  color: var(--white);
  border: none;
  padding: 1rem 2rem;
  border-radius: var(--radius);
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  transition: var(--transition);
  width: 100%;
  justify-content: center;
  margin-top: auto;
}

.btn-contact:hover {
  background: var(--gradient-primary-50);
  transform: translateY(-2px);
  box-shadow: var(--shadow);
}

/* Modal Responsive */
@media (min-width: 768px) {
  .modal-body {
    flex-direction: row;
  }
  
  .modal-image {
    width: 45%;
    height: auto;
    min-height: 100%;
  }
  
  .modal-info {
    width: 55%;
    padding: var(--space-xl);
  }
  
  .btn-contact {
    width: auto;
    align-self: flex-start;
  }
}

/* Transitions */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}
</style>
