<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { ArrowRight } from 'lucide-vue-next';
import '../styles/SupportingServices.css';

const SLIDES = [
  {
    title: 'Supporting Uniformed Services',
    text: 'Total wellness is more than weight management. Partner with Revitalife Compounding Pharmacy to create an all-encompassing, long-term wellness journey.',
    image: '/supporting-services.png',
    label: 'Dedicated Care'
  },
  {
    title: 'Precision Clinical Support',
    text: 'Tailored pharmaceutical solutions for those who serve. We provide the highest quality compounding for military and first responders.',
    image: '/supporting-services-2.png',
    label: 'First Responders'
  },
  {
    title: 'Elite Wellness Journey',
    text: 'Go beyond basic care. Experience a comprehensive health strategy designed specifically for high-intensity uniformed service roles.',
    image: '/supporting-services-3.png',
    label: 'Specialized Support'
  },
  {
    title: 'Long-term Health Partnerships',
    text: 'Building lasting relationships with uniformed personnel to ensure sustainable health, vitality, and peak performance.',
    image: '/supporting-services-4.png',
    label: 'Veteran Care'
  }
];

const currentIndex = ref(0);
const sectionRef = ref(null);
let interval = null;

const handleMouseMove = (e) => {
  if (!sectionRef.value) return;
  const rect = sectionRef.value.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  sectionRef.value.style.setProperty('--mouse-x', `${x}px`);
  sectionRef.value.style.setProperty('--mouse-y', `${y}px`);
};

const nextSlide = () => {
  currentIndex.value = (currentIndex.value + 1) % SLIDES.length;
};

const goToSlide = (index) => {
  currentIndex.value = index;
  resetInterval();
};

const resetInterval = () => {
  if (interval) clearInterval(interval);
  interval = setInterval(nextSlide, 4000);
};

onMounted(() => {
  resetInterval();
});

onUnmounted(() => {
  if (interval) clearInterval(interval);
});
</script>

<template>
  <section 
    ref="sectionRef" 
    class="supporting-services"
    @mousemove="handleMouseMove"
  >
    <div class="container">
      <div class="supporting-grid">
        <div class="supporting-image-wrapper">
          <div class="supporting-carousel-track">
            <TransitionGroup name="fade-slide">
              <div 
                v-for="(slide, index) in SLIDES" 
                :key="index"
                v-show="currentIndex === index"
                class="supporting-image-container"
              >
                <img :src="slide.image" :alt="slide.title" class="supporting-img" />
              </div>
            </TransitionGroup>
            <div class="supporting-image-accent"></div>
          </div>
        </div>

        <div class="supporting-content">
          <Transition name="fade-up" mode="out-in">
            <div :key="currentIndex" class="slide-content">
              <span class="section-label">{{ SLIDES[currentIndex].label }}</span>
              <h2 class="supporting-title">
                {{ SLIDES[currentIndex].title.split(' ')[0] }} 
                <span class="text-gradient">{{ SLIDES[currentIndex].title.split(' ').slice(1).join(' ') }}</span>
              </h2>
              <p class="supporting-text">
                {{ SLIDES[currentIndex].text }}
              </p>
              
              <div class="supporting-actions">
                <a href="#start-prescribing" class="btn btn-primary btn-pill">
                  START PRESCRIBING
                  <ArrowRight :size="18" />
                </a>
              </div>
            </div>
          </Transition>

          <div class="supporting-dots">
            <button 
              v-for="(_, index) in SLIDES" 
              :key="index"
              class="dot"
              :class="{ active: currentIndex === index }"
              @click="goToSlide(index)"
              aria-label="Go to slide"
            ></button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
