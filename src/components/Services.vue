<script setup>
import { Activity, Beaker, Pill, HeartPulse, Brain, ArrowRight, ShieldCheck } from 'lucide-vue-next';
import '../styles/Services.css';

const SERVICES = [
  {
    icon: Activity,
    title: 'Supporting Uniformed Services',
    desc: 'Total wellness is more than weight management. Partner with Revitalife Compounding Pharmacy to create an all-encompassing, long-term wellness journey.',
    link: '#prescribe'
  },
  {
    icon: ShieldCheck,
    title: 'Custom Medical Solutions',
    desc: 'Total wellness is more than weight management. Partner with Revitalife Compounding Pharmacy to create an all-encompassing, long-term wellness journey.',
    link: '#prescribe'
  },
  {
    icon: HeartPulse,
    title: 'Long-term Wellness Journey',
    desc: 'Total wellness is more than weight management. Partner with Revitalife Compounding Pharmacy to create an all-encompassing, long-term wellness journey.',
    link: '#prescribe'
  },
  {
    icon: Beaker,
    title: 'Precision Compounding',
    desc: 'Total wellness is more than weight management. Partner with Revitalife Compounding Pharmacy to create an all-encompassing, long-term wellness journey.',
    link: '#prescribe'
  },
  {
    icon: Pill,
    title: 'Advanced Care Pharmacy',
    desc: 'Total wellness is more than weight management. Partner with Revitalife Compounding Pharmacy to create an all-encompassing, long-term wellness journey.',
    link: '#prescribe'
  },
  {
    icon: Brain,
    title: 'Holistic Health Support',
    desc: 'Total wellness is more than weight management. Partner with Revitalife Compounding Pharmacy to create an all-encompassing, long-term wellness journey.',
    link: '#prescribe'
  }
];
</script>

<template>
  <section class="services" id="services">
    <div class="container">
      <div class="services__header" v-motion-slide-visible-once-bottom>
        <span class="section-label">Our Specialties</span>
        <h2 class="services__title">Comprehensive <span class="text-gradient">Compounding</span></h2>
        <p class="services__subtitle">
          We combine the art and science of pharmacy to create customized medications. 
          Our specialties cover a wide range of medical needs.
        </p>
      </div>

      <div class="services__grid">
        <div 
          v-for="(service, i) in SERVICES" 
          :key="service.title"
          class="service-card"
          v-motion
          :initial="{ opacity: 0, x: 50, scale: 0.95 }"
          :visible-once="{ 
            opacity: 1, 
            x: 0, 
            scale: 1,
            transition: { 
              duration: 1200,
              ease: [0.16, 1, 0.3, 1],
              delay: i * 150 
            } 
          }"
        >
          <div class="service-card__icon-wrapper">
            <component :is="service.icon" :size="32" stroke-width="1.5" />
          </div>
          
          <h3 class="service-card__title">{{ service.title }}</h3>
          <p class="service-card__desc">{{ service.desc }}</p>
          
          <a :href="service.link" class="service-card__link">
            START PRESCRIBING <ArrowRight :size="16" />
          </a>

          <!-- Decorative Elements -->
          <div class="card-number">0{{ i + 1 }}</div>
          <img 
            src="/WhatsApp_Image_2026-05-01_at_12.23.06_AM-removebg-preview.png" 
            alt="Product Accent" 
            class="card-accent-img"
          />
        </div>
      </div>
    </div>
  </section>
</template>
