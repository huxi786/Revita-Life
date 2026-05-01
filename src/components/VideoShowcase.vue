<script setup>
import { Play, ArrowRight } from 'lucide-vue-next';
import '../styles/VideoShowcase.css';

const videos = [
  {
    id: 1,
    title: 'Our Pharmacy',
    desc: 'See how outstanding customer service, pharmaceutical grade quality, and innovative solutions come together to serve our clients.',
    image: '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__4_-removebg-preview.png', // Placeholder
    label: 'SERVICE'
  },
  {
    id: 2,
    title: 'Our Facility',
    desc: 'Watch how North America’s largest compounding facility was built from the ground-up and designed with the industry’s highest quality standards.',
    image: '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__5_-removebg-preview.png', // Placeholder
    label: 'PRECISION'
  },
  {
    id: 3,
    title: 'Working at Revitalife',
    desc: 'Our people define who we are as a company, so we strive to attain, train, and retain talented individuals, who want to make a positive impact in the lives of others.',
    image: '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__6_-removebg-preview.png', // Placeholder
    label: 'CULTURE'
  }
];
</script>

<template>
  <section class="video-showcase section-padding">
    <div class="container">
      <div class="section-header" v-motion-slide-visible-once-bottom>
        <span class="section-label">Inside Revitalife</span>
        <h2 class="section-title">See Our <span class="text-gradient">Story in Motion</span></h2>
        <p class="section-subtitle">Take a virtual tour of our facilities and meet the team behind the care.</p>
      </div>

      <div class="video-grid">
        <div 
          v-for="(video, index) in videos" 
          :key="video.id"
          class="video-card"
          v-motion
          :initial="{ opacity: 0, y: 50 }"
          :visible-once="{ 
            opacity: 1, 
            y: 0, 
            transition: { delay: index * 200, duration: 800 } 
          }"
        >
          <div class="video-card__visual">
            <div class="video-card__img-wrapper">
              <img :src="video.image" :alt="video.title" class="video-card__img" />
              <div class="video-card__overlay"></div>
              <div class="play-button">
                <Play :size="24" fill="currentColor" />
              </div>
            </div>
            <span class="video-card__tag">{{ video.label }}</span>
          </div>

          <div class="video-card__content">
            <h3 class="video-card__title">{{ video.title }}</h3>
            <p class="video-card__desc">{{ video.desc }}</p>
            <a href="#" class="video-card__link">
              Watch Video <ArrowRight :size="16" />
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
