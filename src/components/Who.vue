<template>
    <section id="about">
    <div class="who-content">
        <div class="title">
            <img :src="hero" alt="">
            <h2>Quiénes Somos</h2>
        </div>
        <div class="text">
            <p>En Transpinzon FJ SAS, nos dedicamos a brindar servicios de transporte logístico confiable, rápido y seguro en todo el territorio nacional.</p>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Delectus amet impedit maiores quaerat corporis eligendi et praesentium doloribus id earum at deserunt totam placeat perspiciatis, odit laboriosam quibusdam omnis dignissimos!</p>
        </div>
    </div>
    </section>
</template>

<script setup>
import hero1 from '@/assets/img/placeholders/hero_1.png'

const hero = hero1

</script>

<style scoped>

#about{
    padding: var(--space-lg);
    width: 100%;
    height: auto;
    display: flex;
    justify-content: center;
    align-items: center;
}
.who-content {
    background-color: var(--white);
    overflow: hidden;
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow);
    max-width: 500px;
    max-height: 500px;
    text-align: center;
}
.title{
    max-height: 200px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: relative;
}
.title img{
    width: 100%;
    object-fit:inherit;
}

.title h2 {
    font-size: 2rem;
    margin-bottom: var(--space-md);
    font-family: var(--font-title);
    color: var(--secondary);
    filter: brightness(.9) drop-shadow(0px 0px 10px rgba(0, 0, 0, 0.5));
    font-weight: bolder;
    position: absolute;
    bottom: 0;
    width: 100%;
}

.text {
    padding: var(--space-md);
    overflow: scroll;
    max-height: 300px;
}
p {
    font-size: 1.2rem;
    line-height: 1.6;
    margin-bottom: var(--space-md);
    font-family: var(--font-text);
}

h2::after {
    content: '';
    display: block;
    width: 100%;
    height: 2px;
    background-color: var(--primary);
    margin-top: var(--space-sm);
}

</style>