<script setup>
import { ShieldCheck, Target, ArrowRight, Activity, FlaskConical, Award } from 'lucide-vue-next';
import '../styles/About.css';
import aboutImg from '/WhatsApp Image 2026-05-01 at 12.23.06 AM (1).jpeg';

const FEATURES = [
  {
    icon: ShieldCheck,
    title: 'PCAB Accredited Quality',
    desc: 'We adhere to the highest industry standards for compounding safety and efficacy.'
  },
  {
    icon: FlaskConical,
    title: 'Custom Formulations',
    desc: 'Medications tailored to exact strength, dosage form, and flavor.'
  },
  {
    icon: Activity,
    title: 'Clinical Excellence',
    desc: 'Our pharmacists work directly with your providers for optimal outcomes.'
  }
];
</script>

<template>
  <section class="about section-padding" id="about">
    <div class="container">
      <div class="about__grid">
        <!-- Images / Visual -->
        <div class="about__visual" v-motion-slide-visible-once-left>
          <div class="about__img-wrapper">
            <img 
              :src="aboutImg" 
              alt="Pharmacist compounding medication" 
              class="about__img"
              loading="lazy"
            />
            
            <div 
              class="about__badge glass"
              v-motion
              :initial="{ opacity: 0, scale: 0.8 }"
              :enter="{ opacity: 1, scale: 1, transition: { delay: 500 } }"
              aria-label="PCAB Accredited"
            >
              <div class="about__badge-icon" aria-hidden="true"><Award :size="24" /></div>
              <div>
                <div class="about__badge-label">Accreditation</div>
                <div class="about__badge-value">PCAB Certified</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Content -->
        <div class="about__content" v-motion-slide-visible-once-right>
          <span class="section-label">About Revitalife</span>
          <h2 class="about__title">More Than Just a Pharmacy. <br/><span class="text-gradient">We Are Your Health Partners.</span></h2>
          <p class="about__desc">
            At Revitalife Compounding Pharmacy, we believe medicine shouldn't be one-size-fits-all. 
            Since our founding, we've dedicated ourselves to solving complex medication problems 
            through the art and science of compounding.
          </p>
          
          <ul class="about__features" role="list">
            <li v-for="feat in FEATURES" :key="feat.title" class="about__feature">
              <div class="about__feature-icon-wrapper">
                <component :is="feat.icon" :size="20" class="about__feature-icon" />
              </div>
              <div class="about__feature-text">
                <h3 class="about__feature-title">{{ feat.title }}</h3>
                <p class="about__feature-desc">{{ feat.desc }}</p>
              </div>
            </li>
          </ul>

          <div class="about__actions">
            <a href="#team" class="btn btn-primary">Meet Our Team</a>
            <a href="#facility" class="btn btn-outline">Tour Facility <ArrowRight :size="16" /></a>
          </div>
        </div>
      </div>

      <!-- Gallery Section -->
      <div class="about__gallery" v-motion-slide-visible-once-bottom>
        <div class="gallery__header">
          <span class="section-label">Our Facility</span>
          <h2 class="gallery__title">Where Precision <span class="text-gradient">Meets Care</span></h2>
        </div>
        <div class="gallery__grid">
          <div class="gallery__item" v-for="n in 5" :key="n">
            <img :src="`/WhatsApp Image 2026-05-01 at 12.23.06 AM (${n + 4}).jpeg`" :alt="'Facility image ' + n" class="gallery__img" />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
