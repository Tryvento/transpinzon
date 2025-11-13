<script setup>
import { ref, watch, onMounted } from 'vue';

const props = defineProps({
    sections: {
        type: Array,
        required: true
    },
    activeId: {
        type: String,
        required: true
    }
});

const emit = defineEmits(['change']);

const rotation = ref(0);
const isAnimating = ref(false);

const easeInOutCubic = (t) => {
    return t < 0.5 
        ? 4 * t * t * t 
        : (t - 1) * (2 * t - 2) * (2 * t - 2) + 1;
};

const animateRotation = (targetRotation) => {
    const startRotation = rotation.value;
    // Añadir vueltas completas para hacer la animación más larga
    const additionalRotations = 5 * 360; // 5 vueltas completas adicionales
    const finalTargetRotation = targetRotation + additionalRotations;
    
    const rotationDiff = finalTargetRotation - startRotation;
    const startTime = performance.now();
    const duration = 2000; // 2 segundos para que sea rápida pero con muchas vueltas
    
    isAnimating.value = true;
    
    const animate = (currentTime) => {
        const elapsed = currentTime - startTime;
        const progress = Math.min(elapsed / duration, 1);
        const easedProgress = easeInOutCubic(progress);
        
        rotation.value = startRotation + (rotationDiff * easedProgress);
        
        if (progress < 1) {
            requestAnimationFrame(animate);
        } else {
            isAnimating.value = false;
        }
    };
    
    requestAnimationFrame(animate);
};

watch(
    () => props.activeId,
    (newId, oldId) => {
        if (newId === oldId) return;
        
        const currentIndex = props.sections.findIndex(section => section.id === oldId);
        const newIndex = props.sections.findIndex(section => section.id === newId);
        
        // Calcular la dirección de rotación más corta
        const direction = newIndex > currentIndex ? 1 : -1;
        const rotationStep = 120; // grados por sección
        
        // Rotación base más las vueltas adicionales
        const targetRotation = rotation.value + (rotationStep * direction);
        
        // Animar a la nueva rotación
        animateRotation(targetRotation);
    }
);

const select = (id) => {
    if (isAnimating.value) return;
    emit('change', id);
};
</script>

<template>
    <div class="wheel-nav">
        <div class="wheel-container">
            <!-- Contenedor que recorta la rueda para mostrar solo la mitad -->
            <div class="wheel-clip">
                <div class="wheel" :style="{ transform: `rotate(${rotation}deg)` }">
                    <Icon icon="game-icons:car-wheel" class="wheel-icon" />
                </div>
            </div>
            <div class="controls">
                <button 
                    v-for="(section, index) in sections" 
                    :key="section.id" 
                    @click="select(section.id)" 
                    :class="{
                        'btn': true,
                        'btn-nav': section.id !== activeId,
                        'btn-nav-active': section.id === activeId
                    }"
                    :title="section.content"
                >
                    <Icon :icon="section.icon" class="btn-icon" />
                    <span class="btn-text">{{ section.title }}</span>
                </button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.wheel-nav {
    position: relative;
    transform: translateX(-20%);
    width: 100%;
    z-index: 10;
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 1.5rem;
}

.wheel-container {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 1.5rem;
    width: 100%;
}

/* Contenedor que recorta la rueda para mostrar solo la mitad derecha */
.wheel-clip {
    width: 360px; /* Mitad del ancho original */
    height: 520px;
    overflow: hidden;
    position: relative;
    flex-shrink: 0;
    pointer-events: none;
}

.wheel {
    width: 520px;
    height: 520px;
    will-change: transform;
    transition: transform 0.1s linear; /* Transición lineal para movimiento constante */
    pointer-events: none;
    position: absolute;
    left: -160px; /* Desplazamos la rueda para mostrar solo la mitad derecha */
}

.wheel-icon {
    width: 100%;
    height: 100%;
    object-fit: contain;
    filter: 
        brightness(1.1) contrast(1.1);
    color: var(--secondary);
}

.controls {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    margin: 0 1rem;
    width: 100%;
    max-width: 300px;
}

.btn {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    padding: 0.75rem 1rem;
    border: none;
    border-radius: 50px;
    font-weight: 600;
    font-size: 1.1rem;
    cursor: pointer;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    overflow: hidden;
    white-space: nowrap;
    width: auto;
    min-width: 200px;
    height: 56px;
    margin: 0.5rem 0;
}

.btn-nav {
    background-color: rgba(255, 255, 255, 0.1);
    color: #fff;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
}

.btn-icon {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    margin-right: 12px;
    border-radius: 10px;
    font-size: 1.25rem;
    transition: all 0.3s ease;
    flex-shrink: 0;
}

.btn-nav-active {
    background-color: var(--primary, #004ea7);
    color: white;
    box-shadow: 0 4px 15px rgba(0, 78, 167, 0.3);
    transform: translateX(10px);
}

.btn-nav-active .btn-icon {
    transform: scale(1.1);
}

.btn-nav:hover {
    background-color: rgba(255, 255, 255, 0.15);
    transform: translateX(-4px);
}

.btn-nav-active:hover {
    background-color: var(--primary-hover, #195086);
    transform: translateX(-4px);
}

.btn-text {
    opacity: 1;
    transition: all 0.3s ease;
    white-space: nowrap;
    pointer-events: none;
    font-size: 1rem;
    letter-spacing: 0.5px;
    max-width: 100%;
    overflow: visible;
    padding-right: 1rem;
}

/* Efecto de desgaste en la rueda para hacerla más realista */


/* Responsive adjustments */
@media (max-width: 1024px) {
    .wheel-clip {
        width: 130px;
        height: 260px;
    }
    
    .wheel {
        width: 260px;
        height: 260px;
        left: -130px;
    }
    
    .btn {
        width: 48px;
        height: 48px;
        min-width: 48px;
        padding: 0.5rem;
    }
    
    .btn-icon {
        width: 36px;
        height: 36px;
        font-size: 1.1rem;
        margin: 0;
    }
    
    .btn-nav-active {
        width: 48px;
        padding: 0.5rem;
    }
    
    .wheel-nav {
        order: 2;
    }
    
    .controls {
        flex-direction: row;
        flex-wrap: nowrap;
        justify-content: center;
        max-width: 100%;
        gap: 0.5rem;
        padding: 0 1rem;
    }
    
    .btn {
        margin: 0;
    }
}

@media (max-width: 1023px) {
    .btn {
        min-width: auto;
        width: 48px;
        padding: 0.5rem;
        justify-content: center;
    }
    
    .btn-icon {
        margin: 0;
    }
    
    .btn-text {
        display: none;
    }
    
    .btn-nav-active {
        transform: none;
        width: 48px;
        padding: 0.5rem;
    }
    
    .wheel-nav {
        position: relative;
        width: 100%;
        background: transparent;
        backdrop-filter: none;
        box-shadow: none;
        left: 0;
        transform: none;
    }
    
    .wheel-container {
        flex-direction: row;
        align-items: center;
        gap: 0.75rem;
    }
    
    .wheel-clip {
        width: 80px;
        height: 80px;
    }
    
    .wheel {
        width: 160px;
        height: 160px;
        left: -80px;
    }
    
    .controls {
        flex-direction: row;
        margin: 0;
        gap: 0.4rem;
    }
    
    .btn {
        min-width: 36px;
        width: 36px;
        height: 36px;
        padding: 1.5rem;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    
    .btn-icon {
        font-size: 1rem;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        padding: var(--space-xs);
        background: none;
    }
    
    .btn-text {
        display: none;
    }
    
    .btn-nav-active {
        scale: 1.2;
    }

    
    .btn-nav:hover,
    .btn-nav-active:hover {
        transform: translateY(-2px);
    }
}
</style>