<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { ArrowUpRight, MoveRight } from 'lucide-vue-next';
import '../styles/ProductCategories.css';

const CATEGORIES = [
  { id: '01', title: "Men's Health", image: '/cat-mens.png' },
  { id: '02', title: "Women's Health", image: '/cat-womens.png' },
  { id: '03', title: 'Weight Management', image: '/cat-weight.png' },
  { id: '04', title: 'Longevity', image: '/cat-longevity.png' },
  { id: '05', title: 'Dermatology', image: '/cat-dermatology.png' },
  { id: '06', title: 'Hormone Replacement', image: '/cat-hormone.png' },
  { id: '07', title: 'IV Therapy', image: '/cat-iv.png' },
  { id: '08', title: 'Mental Health', image: '/cat-mental.png' },
];

const sliderRef = ref(null);
const isDragging = ref(false);
const startX = ref(0);
const scrollLeft = ref(0);
const isPaused = ref(false);

// Auto-scroll speed
const speed = 1.5;
let animationFrameId = null;

const startDragging = (e) => {
  isDragging.value = true;
  isPaused.value = true;
  startX.value = (e.pageX || e.touches[0].pageX) - sliderRef.value.offsetLeft;
  scrollLeft.value = sliderRef.value.scrollLeft;
};

const stopDragging = () => {
  isDragging.value = false;
  isPaused.value = false;
};

const dragging = (e) => {
  if (!isDragging.value) return;
  e.preventDefault();
  const x = (e.pageX || e.touches[0].pageX) - sliderRef.value.offsetLeft;
  const walk = (x - startX.value) * 2;
  sliderRef.value.scrollLeft = scrollLeft.value - walk;
};

const animate = () => {
  if (!isPaused.value && !isDragging.value && sliderRef.value) {
    sliderRef.value.scrollLeft += speed;
    
    // Infinite loop trick: reset to start when reaching end
    if (sliderRef.value.scrollLeft >= sliderRef.value.scrollWidth - sliderRef.value.clientWidth) {
      sliderRef.value.scrollLeft = 0;
    }
  }
  animationFrameId = requestAnimationFrame(animate);
};

onMounted(() => {
  animationFrameId = requestAnimationFrame(animate);
});

onUnmounted(() => {
  cancelAnimationFrame(animationFrameId);
});
</script>

<template>
  <section class="product-categories">
    <div class="container">
      <div class="categories-header">
        <h2 class="categories-title">Browse <span class="text-gradient">Product Categories</span></h2>
        <a href="#all-products" class="view-all-btn">
          VIEW ALL PRODUCTS <MoveRight :size="18" />
        </a>
      </div>
    </div>

    <div 
      class="categories-slider-wrapper"
      :class="{ 'is-dragging': isDragging }"
      @mousedown="startDragging"
      @mouseleave="() => { stopDragging(); isPaused = false; }"
      @mouseup="stopDragging"
      @mousemove="dragging"
      @touchstart="startDragging"
      @touchend="stopDragging"
      @touchmove="dragging"
      @mouseenter="isPaused = true"
    >
      <div class="categories-slider" ref="sliderRef">
        <!-- We duplicate the list to make the infinite scroll smoother -->
        <div 
          v-for="(cat, index) in [...CATEGORIES, ...CATEGORIES]" 
          :key="`${cat.id}-${index}`"
          class="category-card"
        >
          <div class="category-card__image">
            <img :src="cat.image" :alt="cat.title" />
            <div class="category-card__overlay"></div>
          </div>
          <div class="category-card__content">
            <div class="category-card__top">
              <span class="category-id">{{ cat.id }}</span>
              <div class="category-arrow">
                <ArrowUpRight :size="20" />
              </div>
            </div>
            <h3 class="category-name">{{ cat.title }}</h3>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
