<template>
  <section id="nosotros" class="about-section">
    <h2 class="section-title">¿Qué ofrecemos?</h2>
    <div class="cards-grid">
      <article class="card" v-for="card in cards" :key="card.titulo">
        <span class="card-icon">{{ card.icon }}</span>
        <h3>{{ card.titulo }}</h3>
        <p>{{ card.desc }}</p>
      </article>
    </div>
  </section>
</template>

<script setup>
const cards = [
  { icon: '🖥️', titulo: 'Desarrollo Web', desc: 'Aprende HTML, CSS y JavaScript desde cero con proyectos reales y mentores experimentados.' },
  { icon: '🤖', titulo: 'Inteligencia Artificial', desc: 'Talleres prácticos sobre machine learning, redes neuronales y uso ético de la IA.' },
  { icon: '🔐', titulo: 'Ciberseguridad', desc: 'Conoce los fundamentos de la seguridad informática y cómo proteger sistemas digitales.' },
]
</script>