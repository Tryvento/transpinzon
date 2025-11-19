<template>
  <section id="services" class="services-section">
    <div class="services-container">
      <!-- Mostrar slider si no hay servicio seleccionado -->
      <div v-if="!selectedService" class="slider-container">
        <button class="slider-arrow left-arrow" @click="prevSlide" aria-label="Anterior servicio">
          <Icon icon="picon:left" class="icon" />
        </button>

        <div
          class="slider-track"
          :style="{ transform: `translateX(-${currentIndex * slideWidth}%)` }"
        >
          <div
            v-for="(service, index) in services"
            :key="index"
            class="service-card"
            :class="{ active: currentIndex === index }"
          >
            <div class="card-image">
              <img :src="service.image" :alt="service.title" loading="lazy" />
            </div>
            <div class="card-content">
              <h3>{{ service.title }}</h3>
              <p>{{ service.description }}</p>
              <button class="btn-details" @click="viewServiceDetails(service)">
                Ver detalles
                <Icon icon="picon:right" class="icon" />
              </button>
            </div>
          </div>
        </div>

        <button class="slider-arrow right-arrow" @click="nextSlide" aria-label="Siguiente servicio">
          <Icon icon="picon:right" class="icon" />
        </button>
      </div>

      <!-- Mostrar detalles del servicio seleccionado -->
      <div v-else class="service-details">
        <button class="btn-back" @click="closeDetails" aria-label="Volver a servicios">
          <Icon icon="picon:left" class="icon" />
          Volver
        </button>
        <div class="details-content">
          <div class="details-image">
            <img :src="selectedService.image" :alt="selectedService.title" />
          </div>
          <h3>{{ selectedService.title }}</h3>
          <p>{{ selectedService.description }}</p>
          <div class="details-extra">
            <p><strong>Características:</strong> {{ selectedService.features }}</p>
            <p><strong>Beneficios:</strong> {{ selectedService.benefits }}</p>
            <p><strong>Precio aproximado:</strong> {{ selectedService.price }}</p>
          </div>
          <button class="btn-contact" @click="contactService">
            Contactar
            <Icon icon="picon:phone" class="icon" />
          </button>
        </div>
      </div>

      <!-- Dots solo si no hay detalles -->
      <div v-if="!selectedService" class="slider-dots">
        <button
          v-for="(_, index) in services"
          :key="index"
          :class="{ active: currentIndex === index }"
          @click="goToSlide(index)"
          :aria-label="`Ir al servicio ${index + 1}`"
        ></button>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed } from 'vue'

// Importar imágenes de ejemplo (reemplaza con tus propias imágenes)
import service1 from '@/assets/img/placeholders/service1.png'
import service2 from '@/assets/img/placeholders/service2.png'
import service3 from '@/assets/img/placeholders/service3.png'
import service4 from '@/assets/img/placeholders/service4.png'

const services = [
  {
    id: 1,
    title: 'Transporte de Carga',
    description:
      'Soluciones logísticas integrales para el transporte seguro de su mercancía a nivel nacional.',
    image: service1,
    features: 'Seguimiento GPS, seguro incluido, entregas puntuales.',
    benefits: 'Eficiencia, seguridad y reducción de costos.',
    price: 'Desde $50.000 COP por envío.',
  },
  {
    id: 2,
    title: 'Distribución Local',
    description:
      'Servicio de distribución eficiente en el área metropolitana con seguimiento en tiempo real.',
    image: service2,
    features: 'Rutas optimizadas, notificaciones en tiempo real, entregas express.',
    benefits: 'Rapidez, confiabilidad y cobertura amplia.',
    price: 'Desde $20.000 COP por distribución.',
  },
  {
    id: 3,
    title: 'Almacenamiento',
    description: 'Espacios seguros y climatizados para el almacenamiento de su mercancía.',
    image: service3,
    features: 'Control de inventario, seguridad 24/7, climatización.',
    benefits: 'Protección de mercancía, organización y acceso fácil.',
    price: 'Desde $10.000 COP por mes.',
  },
  {
    id: 4,
    title: 'Logística Personalizada',
    description: 'Soluciones a medida para las necesidades específicas de su negocio.',
    image: service4,
    features: 'Consultoría logística, planes personalizados, soporte continuo.',
    benefits: 'Adaptación perfecta, optimización de procesos.',
    price: 'Cotización personalizada.',
  },
]

const currentIndex = ref(0)
const selectedService = ref(null)
let autoSlideInterval

// Calcular el ancho del slide basado en el número de cards visibles (siempre 1 para simplificar deslizamiento)
const slideWidth = computed(() => 100) // Siempre 100% para mostrar una card por vez

const nextSlide = () => {
  currentIndex.value = (currentIndex.value + 1) % services.length
}

const prevSlide = () => {
  currentIndex.value = (currentIndex.value - 1 + services.length) % services.length
}

const goToSlide = (index) => {
  currentIndex.value = index
}

const viewServiceDetails = (service) => {
  selectedService.value = service
  if (autoSlideInterval) {
    clearInterval(autoSlideInterval)
  }
}

const closeDetails = () => {
  selectedService.value = null
  startAutoSlide()
}

const contactService = () => {
  // Función para contactar, implementa según necesites
  console.log('Contactar por:', selectedService.value.title)
  // Por ejemplo, abrir un modal o redirigir
}

const startAutoSlide = () => {
  autoSlideInterval = setInterval(() => {
    nextSlide()
  }, 5000) // Cambia cada 5 segundos
}

onMounted(() => {
  startAutoSlide()
})

onBeforeUnmount(() => {
  if (autoSlideInterval) {
    clearInterval(autoSlideInterval)
  }
})
</script>

<style scoped>
.services-section {
  padding: var(--space-md) 0;
  width: 100%;
  max-width: 100%; /* Asegurar que no exceda el contenedor padre */
}

.services-container {
  max-width: 100dvw;
  margin: 0 auto;
  padding: 0 var(--space-sm);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.slider-container {
  position: relative;
  width: 100%;
  max-width: 100%;
  overflow: hidden;
  margin: 0 auto;
  mask-image: linear-gradient(
    to right,
    transparent 0%,
    black 20px,
    black calc(100% - 20px),
    transparent 100%
  );
  -webkit-mask-image: linear-gradient(
    to right,
    transparent 0%,
    black 20px,
    black calc(100% - 20px),
    transparent 100%
  );
  padding: var(--space-sm) 0;
}

.slider-track {
  display: flex;
  transition: transform 0.5s ease-in-out;
  width: 100%;
}

.service-card {
  min-width: 100%;
  max-width: 100%;
  background: var(--white);
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow);
  transition: var(--transition);
  display: flex;
  flex-direction: column;
  margin: 0 var(--space-xs);
  border: 1px solid var(--border);
}

.service-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-hover);
}

.card-image {
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.service-card:hover .card-image img {
  transform: scale(1.05);
}

.card-content {
  padding: var(--space-md);
  flex: 1;
  display: flex;
  flex-direction: column;
}

.card-content h3 {
  color: var(--primary);
  margin-bottom: var(--space-sm);
  font-size: 1.4rem;
  font-weight: 600;
  font-family: var(--font-title);
}

.card-content p {
  color: var(--text-light);
  margin-bottom: var(--space-md);
  line-height: 1.6;
  flex: 1;
  font-family: var(--font-text);
}

.btn-details {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  background: var(--gradient-primary);
  color: white;
  border: none;
  padding: 0.6rem 1.2rem;
  border-radius: var(--radius);
  font-weight: 500;
  cursor: pointer;
  transition: var(--transition);
  margin-top: auto;
  width: 100%;
  max-width: 200px;
  margin: 0 auto;
  font-family: var(--font-title);
}

.btn-details:hover {
  background: var(--gradient-primary-50);
  transform: translateY(-2px);
}

.icon {
  margin-left: 8px;
  transition: transform 0.3s ease;
}

.btn-details:hover .icon {
  transform: translateX(4px);
}

.slider-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: var(--white);
  border: 2px solid var(--primary);
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 10;
  color: var(--primary);
  transition: var(--transition);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.slider-arrow:hover {
  background: var(--primary);
  color: var(--white);
}

.left-arrow {
  left: 10px;
}

.right-arrow {
  right: 10px;
}

.slider-dots {
  display: flex;
  justify-content: center;
  margin-top: var(--space-lg);
  gap: 8px;
}

.slider-dots button {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  border: none;
  background-color: var(--border);
  cursor: pointer;
  padding: 0;
  transition: var(--transition);
}

.slider-dots button.active {
  background: var(--gradient-primary);
  transform: scale(1.2);
}

/* Detalles del servicio */
.service-details {
  width: 100%;
  max-width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: var(--space-md);
}

.btn-back {
  display: flex;
  align-items: center;
  background: var(--gradient-secondary);
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: var(--radius);
  cursor: pointer;
  transition: var(--transition);
  margin-bottom: var(--space-md);
  align-self: flex-start;
}

.btn-back:hover {
  background: var(--gradient-secondary-50);
  transform: translateY(-2px);
}

.btn-back svg {
  margin-right: 8px;
}

.details-content {
  background: var(--white);
  border-radius: var(--radius-lg);
  padding: var(--space-md);
  box-shadow: var(--shadow);
  width: 100%;
  max-width: 600px;
  text-align: center;
}

.details-image {
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: var(--radius);
  margin-bottom: var(--space-md);
}

.details-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.details-content h3 {
  color: var(--primary);
  margin-bottom: var(--space-sm);
  font-size: 1.6rem;
}

.details-content p {
  color: var(--text-light);
  margin-bottom: var(--space-md);
  line-height: 1.6;
}

.details-extra {
  text-align: left;
  margin-bottom: var(--space-md);
}

.details-extra p {
  margin-bottom: var(--space-sm);
}

.btn-contact {
  display: inline-flex;
  align-items: center;
  background: var(--gradient-primary);
  color: white;
  border: none;
  padding: 0.8rem 1.5rem;
  border-radius: var(--radius);
  font-weight: 500;
  cursor: pointer;
  transition: var(--transition);
}

.btn-contact:hover {
  background: var(--gradient-primary-50);
  transform: translateY(-2px);
}

.btn-contact svg {
  margin-left: 8px;
}

/* Responsive Design - Mobile First */
@media (min-width: 480px) {
  .services-section {
    padding: var(--space-lg) 0;
  }

  .card-image {
    height: 250px;
  }

  .details-image {
    height: 250px;
  }

  .card-content h3 {
    font-size: 1.6rem;
  }

  .details-content h3 {
    font-size: 1.8rem;
  }
}

@media (min-width: 768px) {
  .services-container {
    padding: 0 var(--space-md);
  }

  .slider-container {
    padding: var(--space-md) 0;
  }

  .service-card {
    max-width: 500px; /* Limitar ancho para no ser delgado */
  }

  .details-content {
    max-width: 700px;
  }
}

@media (min-width: 1024px) {
  .service-card {
    max-width: 600px;
  }

  .details-content {
    max-width: 800px;
  }
}

/* Animations */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.service-card {
  animation: fadeIn 0.6s ease-out forwards;
  opacity: 0;
}

.service-card:nth-child(1) {
  animation-delay: 0.1s;
}
.service-card:nth-child(2) {
  animation-delay: 0.2s;
}
.service-card:nth-child(3) {
  animation-delay: 0.3s;
}
.service-card:nth-child(4) {
  animation-delay: 0.4s;
}

.service-details {
  animation: fadeIn 0.5s ease-out;
}
</style>
