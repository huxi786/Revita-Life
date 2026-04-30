<script setup>
import { CheckCircle, ShieldCheck, Zap } from 'lucide-vue-next';
import '../styles/WhyRevitalife.css';
</script>

<template>
  <section class="why-revitalife">
    <div class="container">
      <div class="why-grid">
        <div class="why-content" v-motion-slide-visible-once-left>
          <span class="section-label">Superior Standards</span>
          <h2 class="why-title">Why <span class="text-gradient">Revitalife?</span></h2>
          <p class="why-lead">
            We don't just fill prescriptions; we engineer personalized health solutions with uncompromising quality.
          </p>

          <div class="why-features">
            <div class="why-feature">
              <div class="feature-icon">
                <Zap :size="24" />
              </div>
              <div class="feature-text">
                <h3>Technology and Innovation</h3>
                <p>Our state-of-the-art facilities use advanced technology and equipment to produce high-quality, custom medications at scale through our 503A pharmacy.</p>
              </div>
            </div>

            <div class="why-feature">
              <div class="feature-icon">
                <ShieldCheck :size="24" />
              </div>
              <div class="feature-text">
                <h3>Uncompromising Quality</h3>
                <p>Every formulation undergoes rigorous multi-step testing to ensure potency, purity, and absolute safety for every patient.</p>
              </div>
            </div>

            <div class="why-feature">
              <div class="feature-icon">
                <CheckCircle :size="24" />
              </div>
              <div class="feature-text">
                <h3>Precision Scale</h3>
                <p>Combining the personalization of a community pharmacy with the industrial precision of large-scale manufacturing.</p>
              </div>
            </div>
          </div>
        </div>

        <div class="why-media" v-motion-slide-visible-once-right>
          <div class="image-container">
            <img 
              src="/manufacturing-hd.png" 
              alt="Revitalife High-Tech Manufacturing" 
              class="why-image"
            />
            <div class="image-overlay"></div>
          </div>
          
          <div class="why-stats">
            <div class="stat-item">
              <span class="stat-number">503A</span>
              <span class="stat-label">Certified Pharmacy</span>
            </div>
            <div class="stat-divider"></div>
            <div class="stat-item">
              <span class="stat-number">100%</span>
              <span class="stat-label">Custom Formulas</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
