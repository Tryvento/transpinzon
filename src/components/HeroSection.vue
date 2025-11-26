<template>
    <section id="hero">
        <div class="hero-container">
            <div class="slides-container">
                <div 
                    v-for="(image, index) in [currentImage, nextImage]" 
                    :key="image.src"
                    class="slide"
                    :class="{ 'active': index === 0, 'next': index === 1 }"
                    :style="{ backgroundImage: `url(${image.src})` }"
                ></div>
            </div>
            <div class="gradient-overlay"></div>
            <div class="hero-content">
                <h1 class="hero-title">TRANSPINZON FJ SAS</h1>
                <p class="hero-subtitle">Transporte logístico confiable, rápido y seguro en todo el territorio nacional.</p>
                <div class="button-container">
                    <button class="btn btn-primary" @click="scrollToSection('#about')">Conócenos</button>
                    <button class="btn btn-outline" @click="downloadCatalog">Descargar Catálogo</button>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue';
import hero1 from '@/assets/img/placeholders/hero_1.png';
import hero2 from '@/assets/img/placeholders/hero_2.png';
import hero3 from '@/assets/img/placeholders/hero_3.png';
import hero4 from '@/assets/img/placeholders/hero_4.png';
import hero5 from '@/assets/img/placeholders/hero_5.png';
import catalog from '@/assets/files/catalog.pdf'

const heroImages = [
    { src: hero1 },
    { src: hero2 },
    { src: hero3 },
    { src: hero4 },
    { src: hero5 }
];

const currentIndex = ref(0);
let slideInterval;

const downloadCatalog = () => {
    window.open(catalog, '_blank');
};

const currentImage = computed(() => heroImages[currentIndex.value]);
const nextImage = computed(() => {
    const nextIndex = (currentIndex.value + 1) % heroImages.length;
    return heroImages[nextIndex];
});

const nextSlide = async () => {
    // Add fade-out class to current slide
    const activeSlide = document.querySelector('.slide.active');
    const nextSlide = document.querySelector('.slide.next');
    
    if (activeSlide && nextSlide) {
        // Start fade out current and fade in next
        activeSlide.classList.add('fade-out');
        nextSlide.classList.add('fade-in');
        
        // Wait for transition to complete
        await new Promise(resolve => setTimeout(resolve, 1000));
        
        // Update index and reset classes
        currentIndex.value = (currentIndex.value + 1) % heroImages.length;
        
        // Reset classes for next transition
        activeSlide.classList.remove('active', 'fade-out');
        nextSlide.classList.remove('next', 'fade-in');
        nextSlide.classList.add('active');
    }
};

const getRandomInterval = () => {
    return Math.floor(Math.random() * (10000 - 7000 + 1)) + 7000; // Slightly longer interval for better UX
};

const startInterval = () => {
    const interval = getRandomInterval();
    slideInterval = setTimeout(() => {
        nextSlide();
        startInterval();
    }, interval);
};

const scrollToSection = (sectionId) => {
    const element = document.querySelector(sectionId);
    if (element) {
        element.scrollIntoView({ behavior: 'smooth' });
    }
};

onMounted(() => {
    startInterval();
});

onBeforeUnmount(() => {
    if (slideInterval) {
        clearTimeout(slideInterval);
    }
});
</script>

<style scoped>
#hero {
    position: relative;
    width: 100%;
    height: 100vh;
    max-height: 1000px;
    overflow: hidden;
}

.hero-container {
    position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
}

.slides-container {
    position: absolute;
    width: 100%;
    height: 100%;
}

.slide {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    transform: translateZ(0) scale(1);
    backface-visibility: hidden;
    perspective: 1000px;
    will-change: opacity, transform;
    transition: opacity 1s ease-in-out, transform 20s ease-in-out;
    animation: backgroundZoom 20s ease-in-out infinite alternate;
}

.slide.active {
    opacity: 1;
    z-index: 1;
}

.slide.next {
    opacity: 0;
    z-index: 0;
}

.slide.active.fade-out {
    opacity: 0;
}

.slide.next.fade-in {
    opacity: 1;
    z-index: 1;
}

.gradient-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(
        to bottom,
        rgba(0, 0, 0, 0.2) 0%,
        rgba(0, 0, 0, 0.4) 30%,
        rgba(0, 0, 0, 0.6) 60%,
        rgba(0, 0, 0, 0.8) 100%
    );
    z-index: 1;
}

.hero-content {
    position: relative;
    z-index: 2;
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
    color: var(--white);
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    pointer-events: none;
}

.hero-content > * {
    pointer-events: auto;
}

.hero-title {
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: 800;
    margin-bottom: 1.5rem;
    text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.7);
    line-height: 1.2;
    letter-spacing: 2px;
    text-transform: uppercase;
    font-family: var(--font-title);
    animation: fadeInUp 1s ease-out 0.3s both;
}

.hero-subtitle {
    font-size: clamp(1rem, 2vw, 1.5rem);
    margin-bottom: 2.5rem;
    max-width: 800px;
    line-height: 1.6;
    text-shadow: 1px 1px 4px rgba(0, 0, 0, 0.8);
    animation: fadeInUp 1s ease-out 0.6s both;
    font-family: var(--font-text);
}

.button-container {
    display: flex;
    gap: 1.5rem;
    justify-content: center;
    flex-wrap: wrap;
    animation: fadeInUp 1s ease-out 0.9s both;
}

.btn {
    padding: 0.8rem 2rem;
    border-radius: var(--radius);
    font-weight: 600;
    font-size: 1rem;
    cursor: pointer;
    transition: all 0.3s ease;
    text-transform: uppercase;
    letter-spacing: 1px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-width: 180px;
    height: 50px;
    border: 2px solid transparent;
    position: relative;
    overflow: hidden;
}

.btn::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
    transition: left 0.5s ease;
}

.btn:hover::before {
    left: 100%;
}

.btn-primary {
    background-color: var(--secondary);
    color: var(--white);
    border-color: var(--secondary);
}

.btn-primary:hover {
    background-color: var(--secondary-hover);
    transform: translateY(-3px);
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
}

.btn-outline {
    background-color: transparent;
    color: var(--white);
    border: 2px solid var(--white);
    backdrop-filter: blur(10px);
}

.btn-outline:hover {
    background-color: rgba(255, 255, 255, 0.15);
    transform: translateY(-3px);
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
}

/* Animaciones */
@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(30px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes backgroundZoom {
    from {
        transform: scale(1.1) translateZ(0);
    }
    to {
        transform: scale(1) translateZ(0);
    }
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .hero-title {
        font-size: 2.5rem;
        letter-spacing: 1px;
    }
    
    .hero-subtitle {
        font-size: 1.1rem;
        margin-bottom: 2rem;
    }
    
    .btn {
        padding: 0.7rem 1.5rem;
        font-size: 0.9rem;
        min-width: 140px;
        height: 45px;
    }
    
    .button-container {
        gap: 1rem;
    }
}

@media (max-width: 480px) {
    .hero-title {
        font-size: 2rem;
        margin-bottom: 1rem;
    }
    
    .hero-subtitle {
        font-size: 1rem;
        margin-bottom: 1.5rem;
        padding: 0 1rem;
    }
    
    .button-container {
        flex-direction: column;
        width: 100%;
        max-width: 280px;
    }
    
    .btn {
        width: 100%;
    }
    
    .gradient-overlay {
        background: linear-gradient(
            to bottom,
            rgba(0, 0, 0, 0.3) 0%,
            rgba(0, 0, 0, 0.5) 40%,
            rgba(0, 0, 0, 0.8) 100%
        );
    }
}
</style>