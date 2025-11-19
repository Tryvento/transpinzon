<template>
    <section id="contact" class="contact-section">
        <!-- Background elements for depth -->
        <div class="contact-bg-glow"></div>
        
        <div class="contact-container">
            <div class="contact-content-wrapper">
                <form action="" class="contact-form glass-card">
                    <h2>Contáctanos</h2>
                    <div class="input-group">
                        <input type="text" placeholder="Nombre" required>
                    </div>
                    <div class="input-group">
                        <input type="email" placeholder="Email" required>
                    </div>
                    <div class="input-group">
                        <textarea name="" id="" cols="30" rows="5" placeholder="Mensaje" required></textarea>
                    </div>
                    <button type="submit" class="btn btn-primary">
                        <span>Enviar Mensaje</span>
                        <Icon icon="lucide:send" class="btn-icon" />
                    </button>
                </form>

                <div class="contact-info glass-card">
                    <div class="info-header">
                        <h3>Información de Contacto</h3>
                        <p>Estamos listos para atender tus necesidades logísticas.</p>
                    </div>
                    
                    <div class="contact-spam-img">
                        <img src="../assets/img/placeholders/service3.png" alt="Contact Support">
                    </div>

                    <div class="contact-methods">
                        <a href="https://wa.me/573219231178" class="social-btn whatsapp" target="_blank" title="WhatsApp">
                            <Icon icon="mdi:whatsapp" />
                        </a>
                        <a href="mailto:info@transpinzon.com" class="social-btn email" target="_blank" title="Email">
                            <Icon icon="mdi:email" />
                        </a>
                        <a href="https://instagram.com" class="social-btn instagram" target="_blank" title="Instagram">
                            <Icon icon="mdi:instagram" />
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
.contact-section {
    position: relative;
    background-color: var(--primary);
    padding: var(--space-xl) var(--space-lg);
    overflow: hidden;
    min-height: 80vh;
    display: flex;
    align-items: center;
    justify-content: center;
}

.contact-bg-glow {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at center, rgba(0, 78, 167, 0.15) 0%, transparent 70%);
    pointer-events: none;
    z-index: 0;
}

.contact-container {
    width: 100%;
    max-width: 1200px;
    position: relative;
    z-index: 1;
}

.contact-content-wrapper {
    display: grid;
    grid-template-columns: 1.2fr 0.8fr;
    gap: var(--space-xl);
    align-items: stretch;
}

/* Glass Card Styles */
.glass-card {
    background: var(--background);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.05);
    border-radius: var(--radius-lg);
    padding: var(--space-lg);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.glass-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 40px 0 rgba(0, 0, 0, 0.4);
    border-color: rgba(255, 255, 255, 0.1);
}

/* Form Styles */
.contact-form {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}

.contact-form h2 {
    font-family: var(--font-title);
    font-size: 2.5rem;
    color: var(--white);
    margin-bottom: 0.5rem;
    text-transform: uppercase;
    letter-spacing: 1px;
}

.input-group {
    position: relative;
    width: 100%;
}

.contact-form input,
.contact-form textarea {
    width: 100%;
    padding: 1rem;
    background: rgba(0, 0, 0, 0.2);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: var(--radius);
    color: var(--white);
    font-family: var(--font-text);
    font-size: 1rem;
    transition: all 0.3s ease;
}

.contact-form textarea {
    resize: vertical;
    min-height: 150px;
}

.contact-form input:focus,
.contact-form textarea:focus {
    outline: none;
    border-color: var(--secondary);
    background: rgba(0, 0, 0, 0.4);
    box-shadow: 0 0 0 4px rgba(255, 68, 68, 0.1);
}

.contact-form input::placeholder,
.contact-form textarea::placeholder {
    color: var(--text-muted);
}

/* Button Styles */
.btn {
    position: relative;
    padding: 1rem 2rem;
    border-radius: var(--radius);
    border: none;
    cursor: pointer;
    font-family: var(--font-title);
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 1px;
    overflow: hidden;
    transition: all 0.3s ease;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    margin-top: 1rem;
}

.btn-primary {
    background-color: var(--secondary);
    color: var(--white);
    box-shadow: 0 4px 15px rgba(255, 68, 68, 0.3);
}

.btn-primary::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
    transition: left 0.5s ease;
}

.btn-primary:hover::before {
    left: 100%;
}

.btn-primary:hover {
    background-color: var(--secondary-hover);
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(255, 68, 68, 0.4);
}

/* Info Card Styles */
.contact-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    text-align: center;
}

.info-header h3 {
    font-family: var(--font-title);
    color: var(--white);
    font-size: 1.5rem;
    margin-bottom: 0.5rem;
}

.info-header p {
    font-family: var(--font-text);
    color: var(--text-light);
    margin-bottom: 1.5rem;
}

.contact-spam-img {
    width: 100%;
    height: 200px;
    border-radius: var(--radius);
    overflow: hidden;
    margin-bottom: 1.5rem;
    position: relative;
}

.contact-spam-img img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.5s ease;
}

.contact-spam-img:hover img {
    transform: scale(1.1);
}

.contact-methods {
    display: flex;
    justify-content: center;
    gap: 1.5rem;
}

.social-btn {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.05);
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--white);
    font-size: 1.5rem;
    transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    border: 1px solid rgba(255, 255, 255, 0.1);
}

.social-btn:hover {
    transform: translateY(-5px) scale(1.1);
    background: var(--white);
}

.social-btn.whatsapp:hover { color: #25D366; }
.social-btn.email:hover { color: var(--secondary); }
.social-btn.instagram:hover { color: #E1306C; }

/* Responsive */
@media (max-width: 968px) {
    .contact-content-wrapper {
        grid-template-columns: 1fr;
        gap: var(--space-lg);
    }
    
    .contact-spam-img {
        height: 150px;
    }
}

@media (max-width: 480px) {
    .contact-section {
        padding: var(--space-lg) var(--space-sm);
    }
    
    .contact-form h2 {
        font-size: 2rem;
    }
    
    .glass-card {
        padding: var(--space-md);
    }
}
</style>