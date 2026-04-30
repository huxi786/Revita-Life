<script setup>
import '../styles/Hero.css';
</script>

<template>
  <section class="hero" id="home">
    <div class="hero__bg-wrapper">
      <div class="hero__bg-shape-right"></div>
    </div>

    <div class="container hero__container">
      <div class="hero__grid">
        <div class="hero__content" v-motion-slide-visible-once-left>
          <h1 class="hero__title">
            Compounded solutions designed to fit your <i>unique</i> health needs.
          </h1>
          <p class="hero__subtitle">
            Leading the way in personalized medicine through precision, quality, and dedicated pharmaceutical expertise.
          </p>
          
          <div class="hero__actions">
            <a href="#become-provider" class="btn btn-primary btn-pill">
              BECOME A PROVIDER
            </a>
            <a href="#find-provider" class="hero__link-text">
              FIND A PROVIDER
              <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M5 12h14m-7-7l7 7-7 7" />
              </svg>
            </a>
          </div>
        </div>

        <div class="hero__image-wrapper" v-motion-slide-visible-once-right>
          <div class="hero__image-container">
            <img src="/WhatsApp_Image_2026-05-01_at_12.23.06_AM-removebg-preview.png" alt="Revitalife Premium Care" class="hero__main-image" />
          </div>
          

          
        </div>
      </div>
    </div>
  </section>
</template>
