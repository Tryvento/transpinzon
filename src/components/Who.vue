<template>
    <section id="about">
        <div class="who-container">
            <div class="image-side">
                <img :src="hero" alt="Quiénes Somos">
            </div>
            <div class="text-side">
                <div class="content-wrapper">
                    <h2>Quiénes Somos</h2>
                    <p>En Transpinzon FJ SAS, nos dedicamos a brindar servicios de transporte logístico confiable, rápido y seguro en todo el territorio nacional.</p>
                    <p>Contamos con una flota moderna y un equipo humano altamente capacitado para garantizar que su carga llegue a destino en las mejores condiciones y tiempos pactados. Nuestra experiencia nos permite ofrecer soluciones personalizadas para cada tipo de cliente.</p>
                </div>
            </div>
        </div>
    </section>
</template>

<script setup>
import hero1 from '@/assets/img/placeholders/hero_1.png'

const hero = hero1

</script>

<style scoped>
#about {
    width: 100%;
    background-color: var(--white);
    overflow: hidden;
}

.who-container {
    display: flex;
    flex-direction: column;
    width: 100%;
}

/* Mobile First: Image on top, text below */
.image-side {
    width: 100%;
    height: 300px; /* Fixed height for mobile visual anchor */
    overflow: hidden;
}

.image-side img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
}

.text-side {
    width: 100%;
    padding: var(--space-xl) var(--space-lg);
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: var(--white);
}

.content-wrapper {
    max-width: 600px; /* Readable line length */
    width: 100%;
    padding: var(--space-md);
}

h2 {
    font-size: 2.5rem;
    margin-bottom: var(--space-md);
    font-family: var(--font-title);
    color: var(--secondary);
    font-weight: 800;
    position: relative;
    display: inline-block;
}

h2::after {
    content: '';
    display: block;
    width: 100%;
    height: 4px;
    background-color: var(--primary);
    margin-top: var(--space-xs);
    border-radius: 2px;
}

p {
    font-size: 1.1rem;
    line-height: 1.8;
    margin-bottom: var(--space-md);
    font-family: var(--font-text);
    color: var(--text-color); /* Assuming a text color var exists, or fallback to dark gray */
    opacity: 0.9;
}

/* Desktop View */
@media (min-width: 1024px) {
    .who-container {
        flex-direction: row;
        min-height: 600px; /* Substantial height for desktop */
    }

    .image-side {
        width: 50%;
        height: auto; /* Fill container height */
        min-height: 100%;
    }

    .text-side {
        width: 50%;
        padding: var(--space-xxl); /* More breathing room */
        justify-content: flex-start; /* Align text block to the left of its container if needed, or center */
    }
    
    .content-wrapper {
        max-width: 550px; /* Tighter width for better reading on wide screens */
    }

    h2 {
        font-size: 3.5rem;
    }
}
</style>