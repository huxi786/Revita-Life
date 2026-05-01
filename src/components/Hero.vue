<script setup>
import { ref } from 'vue';
import { ChevronRight } from 'lucide-vue-next';
import '../styles/Hero.css';

const images = [
  '/WhatsApp_Image_2026-05-01_at_12.23.06_AM-removebg-preview.png',
  '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__1_-removebg-preview.png',
  '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__2_-removebg-preview.png'
];

const cards = ref([...images]);
const isAnimating = ref(false);

const nextCard = () => {
  if (isAnimating.value) return;
  isAnimating.value = true;
  
  // The first card starts flying out immediately due to 'is-exiting' class
  // After the animation duration, we rearrange the array
  setTimeout(() => {
    const first = cards.value.shift();
    cards.value.push(first);
    isAnimating.value = false;
  }, 700); // Increased duration for a more premium, slower 'throw'
};
</script>

<template>
  <section class="hero" id="home">
    <div class="hero__bg-wrapper">
      <div class="hero__bg-shape-right"></div>
    </div>

    <div class="container hero__container">
      <div class="hero__grid">
        <div class="hero__content" v-motion-slide-visible-once-left>
          <h1 class="hero__title">
            Compounded solutions designed to fit your <i>unique</i> health needs.
          </h1>
          <p class="hero__subtitle">
            Leading the way in personalized medicine through precision, quality, and dedicated pharmaceutical expertise.
          </p>
          
          <div class="hero__actions">
            <a href="#become-provider" class="btn btn-primary btn-pill">
              BECOME A PROVIDER
            </a>
            <a href="#find-provider" class="hero__link-text">
              FIND A PROVIDER
              <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M5 12h14m-7-7l7 7-7 7" />
              </svg>
            </a>
          </div>
        </div>

        <div class="hero__slider-wrapper" v-motion-slide-visible-once-right>
          <div class="hero__stack">
            <div 
              v-for="(img, index) in cards" 
              :key="img"
              class="hero__card"
              :class="{ 'is-exiting': index === 0 && isAnimating }"
              :style="{ 
                zIndex: cards.length - index,
                transform: `translateX(${index * 25}px) translateY(${index * 15}px) rotate(${index * 2}deg) scale(${1 - index * 0.05})`,
                opacity: 1 - index * 0.2
              }"
            >
              <div class="hero__card-inner">
                <img :src="img" alt="Revitalife Product" class="hero__card-image" />
              </div>
            </div>
          </div>

          <button class="hero__slider-next" @click="nextCard" aria-label="Next Slide">
            <ChevronRight :size="32" />
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
