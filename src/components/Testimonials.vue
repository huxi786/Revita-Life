<script setup>
import { Quote } from 'lucide-vue-next';
import '../styles/Testimonials.css';

const TESTIMONIALS = [
  {
    text: "Revitalife changed my life. The commercially available HRT wasn't working for me. My customized compound from Revitalife finally brought me balance.",
    author: 'Sarah M.',
    role: 'Patient',
    stars: 5
  },
  {
    text: "As a physician, I need a compounding pharmacy I can trust implicitly. The team at Revitalife delivers consistent, high-quality medications for my patients every time.",
    author: 'Dr. James R.',
    role: 'Referring Provider',
    stars: 5
  },
  {
    text: "My cat wouldn't take his thyroid medication until Revitalife compounded it into a tuna-flavored treat. Now he thinks he's getting a reward!",
    author: 'Emily T.',
    role: 'Pet Owner',
    stars: 5
  }
];
</script>

<template>
  <section class="testimonials section-padding-lg" id="testimonials">
    <div class="container">
      <div class="testimonials__header" v-motion-slide-visible-once-bottom>
        <span class="section-label">Patient Stories</span>
        <h2 class="testimonials__title">Hear From Our <span class="text-gradient">Community</span></h2>
      </div>

      <div class="testimonials__grid">
        <div 
          v-for="(testimonial, i) in TESTIMONIALS" 
          :key="i"
          class="testimonial-card"
          v-motion
          :initial="{ opacity: 0, y: 30 }"
          :enter="{ opacity: 1, y: 0, transition: { delay: i * 150 } }"
        >
          <Quote class="testimonial-card__icon" :size="32" aria-hidden="true" />
          <div class="testimonial-card__stars" :aria-label="`${testimonial.stars} out of 5 stars`">
            <span v-for="s in testimonial.stars" :key="s" aria-hidden="true">★</span>
          </div>
          <p class="testimonial-card__text">"{{ testimonial.text }}"</p>
          <div class="testimonial-card__author-area">
            <div class="testimonial-card__avatar">{{ testimonial.author.charAt(0) }}</div>
            <div>
              <div class="testimonial-card__author">{{ testimonial.author }}</div>
              <div class="testimonial-card__role">{{ testimonial.role }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
