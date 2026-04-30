<script setup>
import { ref } from 'vue';
import { ArrowRight, Send, Phone, MapPin, Clock, Mail, Award } from 'lucide-vue-next';
import '../styles/CtaFooter.css';
import '../styles/Contact.css';

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
    <!-- Contact Section -->
    <section class="contact section-padding" id="contact">
      <div class="container contact__container">
        
        <div class="contact__info" v-motion-slide-visible-once-left>
          <span class="section-label">Get in Touch</span>
          <h2 class="contact__title">We're Here for <span class="text-gradient-gold">You</span></h2>
          <p class="contact__desc">
            Whether you're a patient looking for a custom formulation or a provider needing a reliable compounding partner, our expert team is ready to assist you.
          </p>

          <div class="contact__methods">
            <div class="contact__method">
              <div class="contact__method-icon-wrapper">
                <Phone class="contact__method-icon" :size="20" />
              </div>
              <div class="contact__method-text">
                <h3>Call Us</h3>
                <a href="tel:+15551234567">(555) 123-4567</a>
              </div>
            </div>

            <div class="contact__method">
              <div class="contact__method-icon-wrapper">
                <Mail class="contact__method-icon" :size="20" />
              </div>
              <div class="contact__method-text">
                <h3>Email</h3>
                <a href="mailto:info@revitaliferx.com">info@revitaliferx.com</a>
              </div>
            </div>

            <div class="contact__method">
              <div class="contact__method-icon-wrapper">
                <MapPin class="contact__method-icon" :size="20" />
              </div>
              <div class="contact__method-text">
                <h3>Location</h3>
                <address>123 Health Blvd, Suite 100<br/>Your City, ST 00000</address>
              </div>
            </div>

            <div class="contact__method">
              <div class="contact__method-icon-wrapper">
                <Clock class="contact__method-icon" :size="20" />
              </div>
              <div class="contact__method-text">
                <h3>Hours</h3>
                <p>Mon–Fri: 9am – 6pm<br/>Sat: 10am – 2pm</p>
              </div>
            </div>
          </div>
        </div>

        <div class="contact__form-wrapper glass" v-motion-slide-visible-once-right>
          <form @submit="handleSubmit" class="contact__form">
            <h3 class="contact__form-title">Send a Message</h3>
            
            <div class="form-group">
              <label for="name">Full Name</label>
              <input 
                type="text" 
                id="name" 
                v-model="formState.name" 
                placeholder="Jane Doe" 
                required 
              />
            </div>

            <div class="form-row">
              <div class="form-group">
                <label for="email">Email Address</label>
                <input 
                  type="email" 
                  id="email" 
                  v-model="formState.email" 
                  placeholder="jane@example.com" 
                  required 
                />
              </div>
              <div class="form-group">
                <label for="phone">Phone Number</label>
                <input 
                  type="tel" 
                  id="phone" 
                  v-model="formState.phone" 
                  placeholder="(555) 000-0000" 
                />
              </div>
            </div>

            <div class="form-group">
              <label for="message">How can we help?</label>
              <textarea 
                id="message" 
                rows="4" 
                v-model="formState.message" 
                placeholder="Please provide details about your inquiry..." 
                required
              ></textarea>
            </div>

            <button type="submit" class="btn btn-primary contact__submit" :disabled="isSubmitting">
              {{ isSubmitting ? 'Sending...' : 'Send Message' }} 
              <Send v-if="!isSubmitting" :size="16" />
            </button>
          </form>
        </div>

      </div>
    </section>

    <!-- Global CTA -->
    <section class="cta-section">
      <div class="container">
        <div class="cta-section__content" v-motion-slide-visible-once-bottom>
          <h2 class="cta-section__title">Ready to Experience Personalized Medicine?</h2>
          <p class="cta-section__subtitle">Transfer your prescription today or have your provider contact us to get started.</p>
          <div class="cta-section__actions">
            <a href="#contact" class="btn btn-gold">
              Get Started <ArrowRight :size="18" />
            </a>
            <a href="tel:+15551234567" class="btn btn-outline-white">
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
              Precision compounding for unique healthcare needs. We collaborate with patients and providers to achieve optimal health outcomes.
            </p>
            <div class="footer__accreditation" aria-label="PCAB Accredited">
              <Award :size="20" class="footer__accreditation-icon" />
              <span class="footer__accreditation-text">PCAB ACCREDITED PHARMACY</span>
            </div>
          </div>

          <div class="footer__links-group">
            <h3 class="footer__title">Quick Links</h3>
            <ul class="footer__list">
              <li><a href="#services">Our Services</a></li>
              <li><a href="#about">About Us</a></li>
              <li><a href="#process">How it Works</a></li>
              <li><a href="#contact">Contact</a></li>
            </ul>
          </div>

          <div class="footer__links-group">
            <h3 class="footer__title">Resources</h3>
            <ul class="footer__list">
              <li><a href="#faq">Patient Resources</a></li>
              <li><a href="#contact">Find a Provider</a></li>
              <li><a href="#">Privacy Policy</a></li>
              <li><a href="#">Terms of Service</a></li>
            </ul>
          </div>

          <div class="footer__contact">
            <h3 class="footer__title">Contact</h3>
            <ul class="footer__list">
              <li>
                <MapPin :size="16" class="footer__icon" />
                <span>123 Health Blvd, Suite 100<br/>Your City, ST 00000</span>
              </li>
              <li>
                <Phone :size="16" class="footer__icon" />
                <a href="tel:+15551234567">(555) 123-4567</a>
              </li>
              <li>
                <Mail :size="16" class="footer__icon" />
                <a href="mailto:info@revitaliferx.com">info@revitaliferx.com</a>
              </li>
            </ul>
          </div>

        </div>

        <div class="footer__bottom">
          <p>&copy; {{ new Date().getFullYear() }} Revitalife Compounding Pharmacy. All rights reserved.</p>
          <div class="footer__socials">
            <a href="#" aria-label="Facebook">Fb</a>
            <a href="#" aria-label="Instagram">Ig</a>
            <a href="#" aria-label="LinkedIn">In</a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>
