<script setup>
import { Activity, Beaker, Pill, HeartPulse, Brain, ArrowRight } from 'lucide-vue-next';
import '../styles/Services.css';

const SERVICES = [
  {
    icon: Activity,
    title: 'Hormone Replacement',
    desc: 'Bio-identical hormone therapy tailored to your exact biological needs for optimal wellness.',
    link: '#hormone'
  },
  {
    icon: Beaker,
    title: 'Dermatology',
    desc: 'Customized topical creams and ointments for anti-aging, acne, and skin conditions.',
    link: '#dermatology'
  },
  {
    icon: HeartPulse,
    title: 'Pain Management',
    desc: 'Targeted pain relief through customized transdermal creams, avoiding systemic side effects.',
    link: '#pain'
  },
  {
    icon: Brain,
    title: 'Mental Health',
    desc: 'Specialized compounding options for depression, anxiety, and cognitive support.',
    link: '#mental-health'
  },
  {
    icon: Pill,
    title: 'Veterinary',
    desc: 'Flavored, easy-to-administer medications created specifically for your furry family members.',
    link: '#veterinary'
  },
  {
    icon: Beaker,
    title: 'Weight Management',
    desc: 'Personalized solutions to support your weight loss journey and metabolic health.',
    link: '#weight-loss'
  }
];
</script>

<template>
  <section class="services section-padding" id="services">
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
          :id="'service-' + service.title.toLowerCase().replace(/\s+/g, '-')"
          class="service-card glass"
          v-motion
          :initial="{ opacity: 0, y: 30 }"
          :enter="{ 
            opacity: 1, 
            y: 0, 
            transition: { 
              duration: 800,
              ease: 'easeOut',
              delay: i * 80 
            } 
          }"
        >
          <div class="service-card__icon-wrapper">
            <component :is="service.icon" :size="24" />
          </div>
          <h3 class="service-card__title">{{ service.title }}</h3>
          <p class="service-card__desc">{{ service.desc }}</p>
          <a :href="service.link" class="service-card__link">
            Learn More <ArrowRight :size="16" />
          </a>
        </div>
      </div>
    </div>
  </section>
</template>
