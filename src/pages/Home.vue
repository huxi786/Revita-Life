<script setup>
import Hero from '../components/Hero.vue';
import Services from '../components/Services.vue';
import About from '../components/About.vue';
import Testimonials from '../components/Testimonials.vue';
import ProcessAndFaq from '../components/ProcessAndFaq.vue';
import ServiceCards from '../components/ServiceCards.vue';
import WhyRevitalife from '../components/WhyRevitalife.vue';
import SupportingServices from '../components/SupportingServices.vue';

const MARQUEE_ITEMS = [
  'Hormone Therapy',
  'Pain Management',
  'Pediatric Compounding',
  'Mental Health',
  'Dermatology',
  'Veterinary Compounding',
  'Nutritional Support',
  'Wound Care',
  'PCAB Accredited',
  'FDA Registered',
  'Third-Party Tested',
];
const doubled = [...MARQUEE_ITEMS, ...MARQUEE_ITEMS];
</script>

<template>
  <main>
    <Hero />
    <div class="marquee-section" aria-hidden="true">
      <div class="marquee-track">
        <div v-for="(item, i) in doubled" :key="i" class="marquee-item">
          <span class="marquee-item__dot"></span>
          {{ item }}
        </div>
      </div>
    </div>
    
    <ServiceCards />
    <WhyRevitalife />
    <SupportingServices />
    
    <Services />
    <About />
    <Testimonials />
    <ProcessAndFaq />
  </main>
</template>
