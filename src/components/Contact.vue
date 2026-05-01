<script setup>
import { Mail, Phone, MapPin, Send, MessageSquare, Clock } from 'lucide-vue-next';
import '../styles/Contact.css';
</script>

<template>
  <section class="contact section-padding" id="contact">
    <div class="container">
      <div class="contact__wrapper glass" v-motion-slide-visible-once-bottom>
        <div class="contact__grid">
          <!-- Contact Info -->
          <div class="contact__info">
            <span class="section-label">Get In Touch</span>
            <h2 class="contact__title">Speak With Our <br/> <span class="text-gradient">Clinical Experts.</span></h2>
            <p class="contact__desc">
              From custom formulations to prescription transfers, our dedicated pharmaceutical team is here to provide the precision care you deserve.
            </p>

            <div class="contact__details">
              <div class="contact__item">
                <div class="contact__item-icon"><Phone :size="20" /></div>
                <div>
                  <h4 class="contact__item-label">Direct Line</h4>
                  <p class="contact__item-value">+1 (555) 000-0000</p>
                </div>
              </div>
              
              <div class="contact__item">
                <div class="contact__item-icon"><Mail :size="20" /></div>
                <div>
                  <h4 class="contact__item-label">General Email</h4>
                  <p class="contact__item-value">hello@revitalife.com</p>
                </div>
              </div>

              <div class="contact__item">
                <div class="contact__item-icon"><Clock :size="20" /></div>
                <div>
                  <h4 class="contact__item-label">Lab Hours</h4>
                  <p class="contact__item-value">Mon - Fri: 9:00 AM - 6:00 PM</p>
                </div>
              </div>
            </div>
          </div>

          <!-- Contact Form -->
          <div class="contact__form-container">
            <form class="contact__form" @submit.prevent>
              <div class="form-group">
                <input type="text" id="name" required placeholder=" " class="form-input" />
                <label for="name" class="form-label">Full Name</label>
              </div>

              <div class="form-grid">
                <div class="form-group">
                  <input type="email" id="email" required placeholder=" " class="form-input" />
                  <label for="email" class="form-label">Email Address</label>
                </div>
                <div class="form-group">
                  <input type="tel" id="phone" placeholder=" " class="form-input" />
                  <label for="phone" class="form-label">Phone (Optional)</label>
                </div>
              </div>

              <div class="form-group">
                <select id="subject" class="form-input">
                  <option value="" disabled selected hidden></option>
                  <option value="general">General Inquiry</option>
                  <option value="prescription">Prescription Transfer</option>
                  <option value="provider">Provider Partnership</option>
                </select>
                <label for="subject" class="form-label">How can we help?</label>
              </div>

              <div class="form-group">
                <textarea id="message" rows="4" required placeholder=" " class="form-input"></textarea>
                <label for="message" class="form-label">Your Message</label>
              </div>

              <button type="submit" class="btn btn-primary btn-block btn-shimmer">
                Send Message <Send :size="18" />
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
