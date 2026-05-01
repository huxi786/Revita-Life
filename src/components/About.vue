<script setup>
import { ShieldCheck, Target, ArrowRight, Activity, FlaskConical, Award } from 'lucide-vue-next';
import '../styles/About.css';
import aboutImg from '/WhatsApp_Image_2026-05-01_at_12.23.06_AM__1_-removebg-preview.png';
const brownBlob = '/brown_blob.png';

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
          <!-- Premium Brown Blobs from Image -->
          <div class="about__blobs-container">
            <!-- Scrolling Background Text (Like the reference photo) -->
            <div class="about__marquee-bg">
              <div class="marquee-text-track">
                <span>REVITALIFE PHARMACY • PRECISION CARE • EXCELLENCE • REVITALIFE PHARMACY • PRECISION CARE • EXCELLENCE • </span>
              </div>
            </div>
          </div>

          <div class="about__img-wrapper">
            <!-- Curved Text on Capsule Cap -->
            <svg class="about__curved-text" viewBox="0 0 400 400">
              <path id="cap-curve" d="M 100, 120 a 100,20 0 1,1 200,0" fill="transparent" />
              <text class="cap-text">
                <textPath xlink:href="#cap-curve" startOffset="50%" text-anchor="middle">
                  QUALITY ASSURED • PRECISION COMPOUNDED
                </textPath>
              </text>
            </svg>

            <div class="about__img-container">
              <img 
                :src="aboutImg" 
                alt="Pharmacist compounding medication" 
                class="about__img"
                loading="lazy"
              />
              <!-- Realistic Reflection -->
              <div class="about__vial-reflection">
                <img :src="aboutImg" alt="" />
              </div>
              
              <div class="about__vial-shadow"></div>
            </div>
            
            <div 
              class="about__badge glass"
              v-motion
              :initial="{ opacity: 0, y: 30, scale: 0.8 }"
              :visible-once="{ 
                opacity: 1, 
                y: 0, 
                scale: 1,
                transition: { delay: 600, duration: 1000, ease: 'easeOut' } 
              }"
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
          <div class="section-label-wrapper">
            <span class="section-label">Our Legacy</span>
          </div>
          <h2 class="about__title">
            Redefining the Standard of <br/>
            <span class="text-gradient">Personalized Wellness.</span>
          </h2>
          <p class="about__desc">
            At Revitalife, we don't just fill prescriptions; we engineer bespoke medical solutions. 
            By blending state-of-the-art pharmaceutical science with a boutique, patient-first approach, 
            we ensure your health journey is as unique as your DNA.
          </p>
          
          <ul class="about__features" role="list">
            <li class="about__feature">
              <div class="about__feature-icon-wrapper">
                <ShieldCheck :size="22" class="about__feature-icon" />
              </div>
              <div class="about__feature-text">
                <h3 class="about__feature-title">Gold Standard Quality</h3>
                <p class="about__feature-desc">Our PCAB-accredited facilities exceed the most rigorous industry benchmarks for safety and precision.</p>
              </div>
            </li>
            <li class="about__feature">
              <div class="about__feature-icon-wrapper">
                <FlaskConical :size="22" class="about__feature-icon" />
              </div>
              <div class="about__feature-text">
                <h3 class="about__feature-title">Pioneering Innovation</h3>
                <p class="about__feature-desc">From rare ingredients to advanced delivery systems, we solve the most complex medication challenges.</p>
              </div>
            </li>
            <li class="about__feature">
              <div class="about__feature-icon-wrapper">
                <Activity :size="22" class="about__feature-icon" />
              </div>
              <div class="about__feature-text">
                <h3 class="about__feature-title">Human-Centric Care</h3>
                <p class="about__feature-desc">A dedicated clinical team that treats you as a partner, ensuring every formulation is optimized for your life.</p>
              </div>
            </li>
          </ul>

          <div class="about__actions">
            <a href="#team" class="btn btn-primary">Our Expertise</a>
            <a href="#facility" class="btn btn-outline">Explore Facility <ArrowRight :size="16" /></a>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>
