<script setup>
import { ArrowRight, ShieldCheck } from 'lucide-vue-next';
import '../styles/ProductShowcase.css';
</script>

<template>
  <section class="product-showcase">
    <!-- Floating Blobs (Background) -->
    <div class="blob-container">
      <div class="blob blob-1" v-motion :initial="{ x: -50, y: -50 }" :enter="{ x: 0, y: 0, transition: { duration: 3000, repeat: Infinity, reverse: true } }"></div>
      <div class="blob blob-2" v-motion :initial="{ x: 50, y: 50 }" :enter="{ x: 0, y: 0, transition: { duration: 4000, repeat: Infinity, reverse: true } }"></div>
      <div class="blob blob-3" v-motion :initial="{ x: -30, y: 30 }" :enter="{ x: 0, y: 0, transition: { duration: 3500, repeat: Infinity, reverse: true } }"></div>
      <div class="blob blob-4" v-motion :initial="{ x: 40, y: -40 }" :enter="{ x: 0, y: 0, transition: { duration: 5000, repeat: Infinity, reverse: true } }"></div>
    </div>

    <div class="container showcase-layout">
      <div class="showcase-content" v-motion-slide-visible-once-left>
        <span class="section-label">Excellence in Pharmacy</span>
        <h2 class="showcase-title">Uncompromising <br /><span class="text-gradient">Quality Standards</span></h2>
        <p class="showcase-desc">
          Every formulation is prepared with surgical precision in our state-of-the-art 
          sterile laboratory, meeting the highest PCAB accreditation standards.
        </p>
        <div class="showcase-stats">
          <div class="stat-item">
            <span class="stat-val">100%</span>
            <span class="stat-label">Third-Party Tested</span>
          </div>
          <div class="stat-item">
            <span class="stat-val">PCAB</span>
            <span class="stat-label">Accredited Lab</span>
          </div>
        </div>
        <button class="btn btn-primary">
          OUR PROCESS <ArrowRight :size="18" />
        </button>
      </div>

      <div class="vial-showcase-wrapper">
        <!-- Background Ticker -->
        <div class="vial-ticker-container">
          <div class="ticker-track">
            <span class="ticker-item">TAILORED WELLNESS • PRECISION CARE • ADVANCED COMPOUNDING • INNOVATIVE FORMULATIONS • REVITALIFE PHARMACY • </span>
            <span class="ticker-item">TAILORED WELLNESS • PRECISION CARE • ADVANCED COMPOUNDING • INNOVATIVE FORMULATIONS • REVITALIFE PHARMACY • </span>
          </div>
        </div>

        <!-- The Badge that pops up from behind the cap -->
        <div class="certification-badge" 
          v-motion 
          :initial="{ y: 50, opacity: 0, scale: 0.8 }"
          :visible-once="{ 
            y: -100, 
            opacity: 1, 
            scale: 1,
            transition: { delay: 500, duration: 1000, ease: 'easeOut' } 
          }"
        >
          <div class="badge-inner">
            <div class="badge-icon">
              <ShieldCheck :size="24" />
            </div>
            <div class="badge-text">
              <span class="badge-pre">ACCREDITATION</span>
              <span class="badge-name">PCAB Certified</span>
            </div>
          </div>
        </div>

        <div class="vial-container">
          <!-- Curved Text on Cap -->
          <svg class="curved-text-svg" viewBox="0 0 400 400">
            <path id="curve" d="M 100, 150 a 100,40 0 1,1 200,0" fill="transparent" />
            <text class="vial-text">
              <textPath xlink:href="#curve" startOffset="50%" text-anchor="middle">
                QUALITY ASSURED • PRECISION COMPOUNDED • QUALITY ASSURED
              </textPath>
            </text>
          </svg>

          <!-- The Large Vial -->
          <div class="vial-img-wrapper" v-motion :initial="{ y: 50 }" :enter="{ y: 0, transition: { duration: 1200, ease: 'easeOut' } }">
            <img src="/WhatsApp_Image_2026-05-01_at_12.23.06_AM__2_-removebg-preview.png" alt="Premium Medication Vial" class="vial-main-img" />
            <div class="vial-shadow"></div>
          </div>

          <!-- Comprehensive Compounding Text Below Bottle -->
          <div class="vial-caption" v-motion-slide-visible-once-bottom>
            <h3 class="vial-sub-title">Comprehensive Compounding</h3>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
