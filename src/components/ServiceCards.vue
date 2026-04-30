<script setup>
import { ref } from 'vue';

const cards = [
  {
    id: 1,
    title: 'Glowing & Radiant Skin',
    subtitle: 'Skin Brightening Therapy',
    description: 'A powerful blend of antioxidants and vitamins designed to detoxify your system and give your skin a natural, luminous glow.',
    image: '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__2_-removebg-preview.png',
    label: 'Aesthetics'
  },
  {
    id: 2,
    title: 'Niacinamide Injection',
    subtitle: 'Vitamin B3 Support',
    description: 'Essential for cellular energy production, skin health, and reducing inflammation for a healthier body and mind.',
    image: '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__3_-removebg-preview.png',
    label: 'Vitality'
  },
  {
    id: 3,
    title: 'Iron Boost Therapy',
    subtitle: 'Energy & Performance',
    description: 'Specially formulated to combat fatigue and restore essential iron levels, boosting your overall energy and performance.',
    image: '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__4_-removebg-preview.png',
    label: 'Wellness'
  },
  {
    id: 4,
    title: 'Energy Boost & Fitness',
    subtitle: 'B-Complex / Carnitine',
    description: 'Optimized cocktail of B-Vitamins and Amino Acids to support high-intensity training and athletic recovery.',
    image: '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__5_-removebg-preview.png',
    label: 'Fitness'
  },
  {
    id: 5,
    title: 'Anti-Stress Support',
    subtitle: 'Antioxidant Support',
    description: 'Vitamin C and Glutamine based formula to help the body manage stress and neutralize free radicals.',
    image: '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__6_-removebg-preview.png',
    label: 'Recovery'
  },
  {
    id: 6,
    title: 'Hydroxocobalamin (B12)',
    subtitle: 'B12 Injection',
    description: 'Pure Vitamin B12 for immediate energy release, improved focus, and neurological health support.',
    image: '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__8_-removebg-preview.png',
    label: 'Essential'
  }
];

const scrollContainer = ref(null);

const scrollNext = () => {
  if (scrollContainer.value) {
    scrollContainer.value.scrollBy({ left: 400, behavior: 'smooth' });
  }
};

const scrollPrev = () => {
  if (scrollContainer.value) {
    scrollContainer.value.scrollBy({ left: -400, behavior: 'smooth' });
  }
};

</script>

<template>
  <section class="service-cards-section">
    <div class="container">
      <div class="section-header" 
        v-motion
        :initial="{ opacity: 0, y: 50 }"
        :visible-once="{ opacity: 1, y: 0, transition: { duration: 1000, ease: 'easeOut' } }"
      >
        <span class="section-label">Our Standards</span>
        <h2 class="section-title">The Future of <span class="text-gradient">Personalized Medicine</span></h2>
        <p class="section-subtitle">Combining cutting-edge technology with pharmaceutical expertise.</p>
      </div>

      <div class="cards-carousel-wrapper">
        <button class="carousel-nav prev" @click="scrollPrev" aria-label="Previous">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M15 18l-6-6 6-6" />
          </svg>
        </button>

        <div class="cards-grid" ref="scrollContainer">
          <div 
            v-for="(card, index) in cards" 
            :key="card.id"
            class="card-wrapper"
            v-motion
            :initial="{ opacity: 0, y: 40 }"
            :visible-once="{ 
              opacity: 1, 
              y: 0,
              transition: { 
                duration: 800,
                ease: 'easeOut',
                delay: index * 100 
              } 
            }"
          >
            <div class="card-glass">
              <div class="card-tag">{{ card.label }}</div>
              
              <div class="card-image-section">
                <img :src="card.image" :alt="card.title" class="card-img" />
                <div class="card-img-glow"></div>
              </div>
              
              <div class="card-info">
                <span class="card-pre">{{ card.subtitle }}</span>
                <h3 class="card-name">{{ card.title }}</h3>
                <p class="card-desc">{{ card.description }}</p>
                
                <div class="card-footer">
                  <span class="explore-link">Explore Details</span>
                  <div class="arrow-circle">
                    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                      <path d="M5 12h14m-7-7l7 7-7 7" />
                    </svg>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <button class="carousel-nav next" @click="scrollNext" aria-label="Next">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M9 18l6-6-6-6" />
          </svg>
        </button>
      </div>
    </div>
  </section>
</template>

<style scoped>
.service-cards-section {
  padding: 160px 0;
  background: radial-gradient(circle at top, #ffffff 0%, #f8f9fb 100%);
  overflow: hidden;
}

.section-header {
  text-align: center;
  margin-bottom: 60px;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

.section-label {
  display: block;
  font-size: 0.85rem;
  font-weight: 800;
  color: var(--color-primary);
  text-transform: uppercase;
  letter-spacing: 0.25em;
  margin-bottom: 0.75rem;
}

.section-title {
  font-family: var(--font-serif);
  font-size: clamp(2.8rem, 5vw, 4.2rem);
  color: var(--color-primary-dark);
  line-height: 1.1;
  margin-bottom: 1rem;
}

.section-subtitle {
  font-size: 1.2rem;
  color: var(--color-text-muted);
  max-width: 600px;
  margin: 0 auto;
}

.cards-carousel-wrapper {
  position: relative;
  width: 100%;
  display: flex;
  align-items: center;
  gap: 20px;
}

.cards-grid {
  display: flex;
  overflow-x: auto;
  scroll-behavior: smooth;
  gap: 30px;
  padding: 40px 10px;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.cards-grid::-webkit-scrollbar {
  display: none;
}

.card-wrapper {
  flex: 0 0 calc((100% - 60px) / 3);
  transform-style: preserve-3d;
  display: flex;
}

.card-glass {
  position: relative;
  background: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-radius: 40px;
  padding: 30px;
  display: flex;
  flex-direction: column;
  flex: 1;
  transition: all 0.6s cubic-bezier(0.2, 0, 0.2, 1);
  box-shadow: 
    0 10px 30px rgba(0, 0, 0, 0.02),
    0 30px 60px rgba(168, 146, 125, 0.05);
}

.card-glass:hover {
  transform: translateY(-12px);
  background: #ffffff;
  box-shadow: 
    0 15px 35px rgba(0, 0, 0, 0.04),
    0 40px 80px rgba(168, 146, 125, 0.1);
  border-color: var(--color-primary-light);
}

.card-tag {
  position: absolute;
  top: 40px;
  right: 40px;
  padding: 8px 16px;
  background: var(--color-primary-light);
  color: var(--color-primary-dark);
  font-size: 0.7rem;
  font-weight: 800;
  border-radius: 100px;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  z-index: 5;
}

.carousel-nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: #ffffff;
  border: 1px solid var(--color-border);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-primary);
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  z-index: 20;
}

.carousel-nav.prev {
  left: -70px;
}

.carousel-nav.next {
  right: -70px;
}

.carousel-nav:hover {
  background: var(--color-primary);
  color: #fff;
  transform: translateY(-50%) scale(1.1);
}

.card-image-section {
  position: relative;
  height: 250px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 25px;
}

.card-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  transition: transform 0.6s ease;
  filter: drop-shadow(0 25px 40px rgba(0,0,0,0.15));
}

.card-glass:hover .card-img {
  transform: scale(1.1) rotate(5deg);
}

.card-img-glow {
  position: absolute;
  width: 150px;
  height: 150px;
  background: radial-gradient(circle, var(--color-primary-light) 0%, transparent 70%);
  opacity: 0.3;
  z-index: -1;
}

.card-info {
  display: flex;
  flex-direction: column;
}

.card-pre {
  font-size: 0.8rem;
  font-weight: 700;
  color: var(--color-primary);
  text-transform: uppercase;
  letter-spacing: 0.15em;
  margin-bottom: 1rem;
}

.card-name {
  font-family: var(--font-serif);
  font-size: 1.6rem;
  color: var(--color-primary-dark);
  margin-bottom: 1rem;
  line-height: 1.2;
}

.card-desc {
  font-size: 0.95rem;
  color: var(--color-text-muted);
  line-height: 1.6;
  margin-bottom: 1.5rem;
  min-height: 4em;
}

.card-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: auto;
}

.explore-link {
  font-weight: 800;
  font-size: 0.9rem;
  color: var(--color-primary-dark);
  letter-spacing: 0.05em;
}

.arrow-circle {
  width: 48px;
  height: 48px;
  background: var(--color-primary-dark);
  color: #ffffff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.card-glass:hover .arrow-circle {
  background: var(--color-primary);
  transform: rotate(-45deg);
}

@media (max-width: 1200px) {
  .card-wrapper {
    flex: 0 0 calc((100% - 30px) / 2);
  }
  
  .carousel-nav.prev { left: -30px; }
  .carousel-nav.next { right: -30px; }
}

@media (max-width: 768px) {
  .card-wrapper {
    flex: 0 0 100%;
  }
  
  .carousel-nav {
    display: none; /* Hide buttons on mobile for touch swipe */
  }
  
  .card-glass {
    padding: 30px;
    border-radius: 32px;
  }
  
  .card-name {
    font-size: 1.7rem;
  }
}
</style>

