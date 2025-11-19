<template>
  <div class="app-layout">
    <header :class="{ 'scrolled': scroll > 50 }">
      <div class="container">
        <div class="header-content">
          <div class="logo">
            <img 
              src="../assets/img/main_logo.png" 
              alt="Transpinzon Logo"
              class="logo-img"
            >
            <h1 class="logo-text">Transpinzon</h1>
          </div>
          
          <!-- Mobile menu button -->
          <button 
            class="menu-toggle"
            @click="toggleMenu"
            aria-label="Toggle menu"
          >
            <Icon :icon="isMenuOpen ? 'tabler:x' : 'ep:menu'" class="menu-icon" />
          </button>
          
          <!-- Navigation -->
          <nav :class="{ 'active': isMenuOpen }">
            <ul class="nav-list">
              <li class="nav-item">
                <a href="#hero" class="nav-link" @click="(e) => scrollToSection(e, '#hero')">Inicio</a>
              </li>
              <li class="nav-item">
                <a href="#about" class="nav-link" @click="(e) => scrollToSection(e, '#about')">Conocenos</a>
              </li>
              <li class="nav-item">
                <a href="#contact" class="nav-link" @click="(e) => scrollToSection(e, '#contact')">Contacto</a>
              </li>
            </ul>
          </nav>
        </div>
      </div>
    </header>
    
    <main class="main-content">
      <slot />
    </main>
    
    <footer class="footer">
      <div class="container">
        <p>&copy; 2025 Transpinzon. Todos los derechos reservados.</p>
        <p class="disclaimer">Esta página es una primera entrega sujeta a cambios y modificaciones. Tanto los textos como las imágenes son de prueba para el desarrollo.</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const scroll = ref(0);
const isMenuOpen = ref(false);

const handleScroll = () => {
  scroll.value = window.scrollY;
};

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
  document.body.style.overflow = isMenuOpen.value ? 'hidden' : '';
};

const scrollToSection = (event, sectionId) => {
  event.preventDefault();
  const element = document.querySelector(sectionId);
  if (element) {
    // Close mobile menu if open
    if (isMenuOpen.value) {
      isMenuOpen.value = false;
      document.body.style.overflow = '';
    }
    
    // Calculate the position to scroll to (accounting for fixed header)
    const headerOffset = 80; // Height of your header
    const elementPosition = element.getBoundingClientRect().top;
    const offsetPosition = elementPosition + window.pageYOffset - headerOffset;

    window.scrollTo({
      top: offsetPosition,
      behavior: 'smooth'
    });
  }
};

const handleResize = () => {
  if (window.innerWidth >= 1024) {
    isMenuOpen.value = false;
    document.body.style.overflow = '';
  }
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
  window.removeEventListener('resize', handleResize);
  document.body.style.overflow = '';
});
</script>

<style scoped>
/* Base Styles (Mobile First) */
.app-layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--space-md);
}

/* Header */
header {
  background: var(--gradient-primary);
  color: var(--white);
  padding: var(--space-sm) 0;
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 1000;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  transition: var(--transition);
}

header.scrolled {
  padding: var(--space-xs) 0;
  background: var(--primary);
  box-shadow: var(--shadow);
}

header.scrolled .logo-text {
  opacity: 1;
  transform: translateX(0);
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
}

/* Logo */
.logo {
  display: flex;
  align-items: center;
  gap: var(--space-sm);
  z-index: 1001;
  flex-shrink: 0;
}

.logo-img {
  width: 40px;
  height: 40px;
  object-fit: contain;
  transition: var(--transition);
  background-color: var(--white);
  padding: var(--space-xs);
  border-radius: var(--radius);
  scale: 1.2;
  border: 2px solid transparent;
}

.logo-text {
  font-family: var(--font-title);
  font-size: 1.5rem;
  font-weight: 700;
  margin: 0;
  background: linear-gradient(to right, var(--white), #f0f0f0);
  background-clip: text;
  -webkit-text-fill-color: transparent;
  opacity: 0;
  transform: translateX(-10px);
  transition: var(--transition);
}

.scrolled .logo-text {
  opacity: 1;
  transform: translateX(0);
}

/* Mobile Menu Toggle */
.menu-toggle {
  background: none;
  border: none;
  color: var(--white);
  font-size: 1.8rem;
  cursor: pointer;
  padding: var(--space-xs);
  z-index: 1001;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: var(--transition);
  flex-shrink: 0;
}

.menu-toggle.hidden {
  opacity: 0;
  visibility: hidden;
  width: 0;
}

.menu-icon {
  transition: var(--transition);
}

/* Navigation */
nav {
  transition: all 0.3s ease;
}

.nav-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: var(--space-md);
  margin-bottom: var(--space-md);
  font-family: var(--font-text);
}

.nav-link {
  color: var(--white);
  text-decoration: none;
  font-weight: 500;
  transition: var(--transition);
  padding: var(--space-xs) 0;
  display: block;
  position: relative;
  font-size: 1.1rem;
  height: 100%;
  display: flex;
  align-items: center;
}

.nav-link::after {
  content: '';
  position: absolute;
  width: 0;
  height: 2px;
  bottom: 0;
  left: 0;
  background-color: var(--secondary);
  transition: var(--transition);
}

.nav-link:hover::after {
  width: 100%;
}

/* Main Content */
.main-content {
  flex: 1;
  padding: var(--space-lg) 0;
}

/* Footer */
.footer {
  background: var(--dark);
  color: var(--white);
  padding: var(--space-md) 0;
  text-align: center;
  margin-top: auto;
  font-family: var(--font-text);
}

.disclaimer {
  font-size: 0.85rem;
  color: var(--text-muted);
  margin-top: var(--space-sm);
  opacity: 0.8;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

/* Mobile Menu */
@media (max-width: 1023px) {
  nav {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background: var(--primary);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    transform: translateY(-100%);
    opacity: 0;
    visibility: hidden;
    transition: var(--transition);
    z-index: 1000;
    padding: var(--space-xl);
    text-align: center;
  }

  nav.active {
    transform: translateY(0);
    opacity: 1;
    visibility: visible;
  }

  nav.hidden {
    display: none;
  }
}

/* Desktop Styles */
@media (min-width: 1024px) {
  .menu-toggle {
    display: none;
  }
  
  .search-toggle-desktop {
    display: flex;
    align-items: center;
    justify-content: center;
    order: 3; /* Para posicionarlo a la derecha */
  }
  
  nav {
    display: flex;
    align-items: center;
    margin-left: auto;
    margin-right: var(--space-lg);
    order: 2; /* Para posicionarlo en el centro */
    opacity: 1;
    visibility: visible;
    transform: none;
    position: static;
    width: auto;
    height: auto;
    background: transparent;
    padding: 0;
  }
  
  .nav-list {
    flex-direction: row;
    margin-bottom: 0;
    gap: var(--space-lg);
  }
  
  .nav-link {
    padding: var(--space-xs) 0;
    font-size: 1rem;
  }

  /* Ocultar navegación cuando la búsqueda está activa */
  nav.nav-hidden {
    opacity: 0;
    visibility: hidden;
    width: 0;
    margin: 0;
    transform: scale(0.8);
  }
  
  .nav-hidden .nav-list {
    display: none;
  }
  
  
  .search-container-desktop {
    display: block;
    order: 4; /* Para posicionarlo al final */
  }
  
  .logo:hover .logo-img {
    transform: rotate(10deg);
    border: 2px solid var(--secondary);
  }
  
  .logo:hover .logo-text {
    transform: translateX(5px);
  }

  /* Ajustar el header content para el nuevo layout */
  .header-content {
    justify-content: flex-start;
    gap: var(--space-lg);
  }
}

/* Large Desktop Styles */
@media (min-width: 1280px) {
  .container {
    padding: 0 var(--space-lg);
  }
  
  .logo-text {
    font-size: 2rem;
  }
  
  .nav-link {
    font-size: 1.1rem;
  }
  
  .search-container-desktop.active {
    width: 450px;
  }
}
</style>