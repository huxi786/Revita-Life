<script setup>
import Hero from '../components/Hero.vue';
import Services from '../components/Services.vue';
import About from '../components/About.vue';
import VideoShowcase from '../components/VideoShowcase.vue';
import Contact from '../components/Contact.vue';
import ServiceCards from '../components/ServiceCards.vue';
import WhyRevitalife from '../components/WhyRevitalife.vue';
import SupportingServices from '../components/SupportingServices.vue';
import ProductCategories from '../components/ProductCategories.vue';
</script>

<template>
  <main>
    <Hero />
    <ServiceCards />
    <ProductCategories />
    <WhyRevitalife />
    <SupportingServices />
    <Services />
    <About />
    <VideoShowcase />
    <Contact />
  </main>
</template>
