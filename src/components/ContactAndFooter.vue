<script setup>
import { ref, onMounted } from 'vue';
import { useIntersectionObserver } from '@vueuse/core';
import { ArrowRight, Send, Phone, MapPin, Clock, Mail, Award } from 'lucide-vue-next';
import '../styles/CtaFooter.css';
import '../styles/Contact.css';

const target = ref(null);
const typedTitle = ref('');
const typedSubtitle = ref('');
const isTypingTitle = ref(false);
const isTypingSubtitle = ref(false);
const fullTitle = 'Ready to Experience Personalized Medicine?';
const fullSubtitle = 'Transfer your prescription today or have your provider contact us to get started.';
const hasStartedTyping = ref(false);

const typeText = async (text, refTarget, speed = 40, flagRef = null) => {
  if (flagRef) flagRef.value = true;
  for (let i = 0; i < text.length; i++) {
    refTarget.value += text[i];
    await new Promise(resolve => setTimeout(resolve, speed));
  }
  if (flagRef) flagRef.value = false;
};

useIntersectionObserver(target, ([{ isIntersecting }]) => {
  if (isIntersecting && !hasStartedTyping.value) {
    hasStartedTyping.value = true;
    startAnimations();
  }
});

const startAnimations = async () => {
  await typeText(fullTitle, typedTitle, 30, isTypingTitle);
  await typeText(fullSubtitle, typedSubtitle, 20, isTypingSubtitle);
};

const formState = ref({
  name: '',
  email: '',
  phone: '',
  message: ''
});
const isSubmitting = ref(false);

const handleSubmit = (e) => {
  e.preventDefault();
  isSubmitting.value = true;
  // Simulate API call
  setTimeout(() => {
    isSubmitting.value = false;
    alert("Thank you for your message! Our team will contact you shortly.");
    formState.value = { name: '', email: '', phone: '', message: '' };
  }, 1500);
};
</script>

<template>
  <div>
    <!-- Global CTA -->
    <section class="cta-section" ref="target">
      <div class="container">
        <div class="cta-section__content">
          <h2 class="cta-section__title">{{ typedTitle }}<span class="cursor" v-if="isTypingTitle">|</span></h2>
          <p class="cta-section__subtitle">{{ typedSubtitle }}<span class="cursor" v-if="isTypingSubtitle">|</span></p>
          <div 
            class="cta-section__actions"
            v-motion
            :initial="{ opacity: 0, y: 20 }"
            :visible-once="{ 
              opacity: 1, 
              y: 0, 
              transition: { delay: 2500, duration: 800 } 
            }"
          >
            <a href="#contact" class="btn btn-gold btn-xl">
              Get Started <ArrowRight :size="22" />
            </a>
            <a href="tel:+15551234567" class="btn btn-outline-white btn-xl">
              Call Pharmacist
            </a>
          </div>
        </div>
      </div>
    </section>

    <!-- Global Footer -->
    <footer class="footer">
      <div class="container">
        <div class="footer__grid">
          
          <div class="footer__brand">
            <a href="#" class="footer__logo" aria-label="Revitalife Pharmacy Home">
              <img 
                src="https://revitalifecompoundingpharmacy.com/wp-content/uploads/2025/12/revita-life-logo.png" 
                alt="Revitalife Compounding Pharmacy"
                style="height: 45px; width: auto; filter: brightness(0) invert(1);"
              />
            </a>
            <p class="footer__desc">
              We are a compounding pharmacy that uses state-of-the-art technology to prepare quality custom-tailored medications. Our team is dedicated to researching and developing personalized, effective solutions for your medication challenges.
            </p>
            <div class="footer__accreditation">
              <Award :size="20" class="footer__icon" />
              <span class="footer__accreditation-text">PCAB ACCREDITED PHARMACY</span>
            </div>
          </div>

          <div class="footer__links-group">
            <h3 class="footer__title">Quick Links</h3>
            <ul class="footer__list">
              <li><a href="#">Providers</a></li>
              <li><a href="#">Compounded Preparations</a></li>
              <li><a href="#">Become a Provider</a></li>
            </ul>
          </div>

          <div class="footer__links-group">
            <h3 class="footer__title">Company</h3>
            <ul class="footer__list">
              <li><a href="#">Support</a></li>
              <li><a href="#">Privacy Policy</a></li>
            </ul>
          </div>

          <div class="footer__contact">
            <h3 class="footer__title">Contact Us</h3>
            <ul class="footer__list">
              <li>
                <MapPin :size="18" class="footer__icon" />
                <span>Dubai Digital Park, Building A7 UG (Upper Ground Floor) 49 – Dubai Silicon Oasis – Dubai</span>
              </li>
              <li>
                <Mail :size="18" class="footer__icon" />
                <a href="mailto:info@revitalifecompoundingpharmacy.com">info@revitalifecompoundingpharmacy.com</a>
              </li>
              <li>
                <Phone :size="18" class="footer__icon" />
                <div style="display: flex; flex-direction: column;">
                  <a href="tel:+97145572762">+971 4 557 2762</a>
                  <a href="tel:+97145525032">+971 4 552 5032</a>
                </div>
              </li>
            </ul>
          </div>

        </div>

        <div class="footer__bottom">
          <div class="footer__disclaimer">
            <p><strong>Disclaimer:</strong> The Service will be limited to Healthcare Professionals Only and not valid for General Public.</p>
          </div>
          <div class="footer__socials">
            <a href="#" aria-label="Facebook">FB</a>
            <a href="#" aria-label="Instagram">IG</a>
            <a href="#" aria-label="LinkedIn">IN</a>
          </div>
        </div>
        
        <div style="margin-top: 20px; text-align: center; font-size: 0.75rem; opacity: 0.5;">
          <p>&copy; {{ new Date().getFullYear() }} Revitalife Compounding Pharmacy. All rights reserved.</p>
        </div>
      </div>
    </footer>
  </div>
</template>
