<script setup>
import { MapPin, Phone, Mail, Clock } from 'lucide-vue-next';
</script>

<template>
  <main class="locations-page">
    <section class="locations-hero">
      <div class="container text-center">
        <h1 class="locations-hero__title">Find a Revitalife Location</h1>
        <p class="locations-hero__subtitle">We are here to serve your compounding needs with expert care and precision.</p>
      </div>
    </section>

    <section class="locations-content section-padding">
      <div class="container">
        <div class="locations-grid">
          <!-- Main Location Card -->
          <div class="location-card glass">
            <h2 class="location-card__title">Revitalife Pharmacy - Main Hub</h2>
            <div class="location-card__details">
              <div class="detail-item">
                <MapPin class="detail-icon" :size="20" />
                <address>123 Health Blvd, Suite 100<br/>Your City, ST 00000</address>
              </div>
              <div class="detail-item">
                <Phone class="detail-icon" :size="20" />
                <a href="tel:+15551234567">(555) 123-4567</a>
              </div>
              <div class="detail-item">
                <Mail class="detail-icon" :size="20" />
                <a href="mailto:info@revitaliferx.com">info@revitaliferx.com</a>
              </div>
              <div class="detail-item">
                <Clock class="detail-icon" :size="20" />
                <p>Mon–Fri: 9:00 AM – 6:00 PM<br/>Sat: 10:00 AM – 2:00 PM<br/>Sun: Closed</p>
              </div>
            </div>
            <a href="#" class="btn btn-primary location-btn">Get Directions</a>
          </div>

          <!-- Coming Soon Location Card -->
          <div class="location-card glass">
            <h2 class="location-card__title">Revitalife Pharmacy - North</h2>
            <div class="location-card__details">
              <div class="detail-item">
                <MapPin class="detail-icon" :size="20" />
                <address>456 Wellness Way, Suite 200<br/>New City, ST 00000</address>
              </div>
              <div class="detail-item">
                <Clock class="detail-icon" :size="20" />
                <p class="text-gradient">Opening Soon - Fall 2026</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped>
.locations-page {
  padding-top: 120px;
  min-height: 80vh;
}
.locations-hero {
  padding: 4rem 0 2rem;
}
.locations-hero__title {
  font-size: clamp(2.5rem, 5vw, 4rem);
  color: var(--color-primary);
  margin-bottom: 1rem;
}
.locations-hero__subtitle {
  font-size: 1.25rem;
  color: var(--color-text-muted);
}
.locations-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}
.location-card {
  padding: 2.5rem;
  border-radius: 20px;
  background: var(--color-white);
  box-shadow: 0 10px 30px rgba(0,0,0,0.05);
}
.location-card__title {
  font-size: 1.5rem;
  color: var(--color-primary-dark);
  margin-bottom: 1.5rem;
  border-bottom: 1px solid var(--color-border);
  padding-bottom: 1rem;
}
.location-card__details {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
  margin-bottom: 2rem;
}
.detail-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  color: var(--color-text);
  line-height: 1.5;
}
.detail-icon {
  color: var(--color-primary);
  flex-shrink: 0;
  margin-top: 2px;
}
.detail-item a {
  color: var(--color-text);
  text-decoration: none;
  transition: color 0.2s ease;
}
.detail-item a:hover {
  color: var(--color-primary);
}
.location-btn {
  width: 100%;
  text-align: center;
}
</style>
