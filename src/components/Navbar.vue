<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue';
import { MapPin, Search, User, ArrowUpRight } from 'lucide-vue-next';
import SearchModal from './SearchModal.vue';
import '../styles/Navbar.css';

const NAV_LINKS = [
  {
    label: 'PROVIDERS',
    href: '#providers',
    hasMega: true,
  },
  {
    label: 'PATIENTS',
    href: '#patients',
    hasMega: true,
  },
  {
    label: 'COMPOUNDED PREPARATIONS',
    href: '#compounded-preparations',
    hasMega: true,
  },
  { label: 'SUPPORT', href: '#support', hasMega: true },
  { label: 'RESOURCES', href: '#resources', hasMega: true },
  { label: 'COMPANY', href: '#company', hasMega: true },
];

const TOP_BAR_PHRASES = [
  "Custom Compounding",
  "Personalized Care",
  "Highest Quality Standards",
  "Your Health, Tailored",
];

const scrolled = ref(false);
const menuOpen = ref(false);
const isSearchOpen = ref(false);
const currentPhraseIndex = ref(0);
const activeMega = ref(null);
const activeMobileMega = ref(null);
let megaCloseTimer = null;

let phraseInterval;

const handleScroll = () => {
  scrolled.value = window.scrollY > 10;
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true });
  phraseInterval = setInterval(() => {
    currentPhraseIndex.value = (currentPhraseIndex.value + 1) % TOP_BAR_PHRASES.length;
  }, 2000);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
  clearInterval(phraseInterval);
});

watch(menuOpen, (newVal) => {
  document.body.style.overflow = newVal ? 'hidden' : '';
  if (!newVal) activeMobileMega.value = null; // reset when closing
});

const handleLinkClick = (link) => {
  if (window.innerWidth < 1024 && link.hasMega) {
    activeMobileMega.value = activeMobileMega.value === link.label ? null : link.label;
    return;
  }
  menuOpen.value = false;
};

const openMega = (label) => {
  clearTimeout(megaCloseTimer);
  activeMega.value = label;
};

const closeMega = () => {
  megaCloseTimer = setTimeout(() => {
    activeMega.value = null;
  }, 150);
};
</script>

<template>
  <header :class="['header-wrapper', { 'is-scrolled': scrolled }]">
    <!-- Top Bar -->
    <div class="top-bar">
      <div class="top-bar__left">
        <router-link to="/locations" class="top-bar__link">
          <MapPin :size="14" /> FIND A REVITALIFE LOCATION
        </router-link>
      </div>
      <div class="top-bar__center">
        <transition name="slide-up" mode="out-in">
          <span :key="currentPhraseIndex" class="top-bar__announcement">
            {{ TOP_BAR_PHRASES[currentPhraseIndex] }}
          </span>
        </transition>
      </div>
      <div class="top-bar__right">
        <button class="top-bar__icon-btn" aria-label="Search" @click="isSearchOpen = true">
          <Search :size="16" />
        </button>
        <button class="top-bar__icon-btn" aria-label="User Account">
          <User :size="16" />
        </button>
      </div>
    </div>

    <!-- Main Navbar -->
    <nav class="navbar" aria-label="Main navigation">
      <div class="navbar__inner">
        <!-- Logo -->
        <router-link to="/" class="navbar__logo" aria-label="Revitalife Pharmacy Home">
          <img
            src="https://revitalifecompoundingpharmacy.com/wp-content/uploads/2025/12/revita-life-logo.png"
            alt="Revitalife Compounding Pharmacy"
            style="height: 45px; width: auto;"
          />
        </router-link>

        <!-- Desktop Nav -->
        <ul class="navbar__nav" role="list">
          <li
            v-for="link in NAV_LINKS"
            :key="link.label"
            class="navbar__nav-item"
            @mouseenter="link.hasMega ? openMega(link.label) : null"
            @mouseleave="link.hasMega ? closeMega() : null"
          >
            <a
              :href="link.href"
              :class="['navbar__link', { 'is-active': activeMega === link.label }]"
            >
              {{ link.label }}
              <span class="navbar__link-icon">{{ activeMega === link.label ? '−' : '+' }}</span>
              <span class="navbar__link-underline"></span>
            </a>

            <!-- MEGA MENU -->
            <div
              v-if="link.hasMega"
              :class="['mega-menu', { 'mega-menu--open': activeMega === link.label }]"
              @mouseenter="openMega(link.label)"
              @mouseleave="closeMega()"
            >
              <!-- PROVIDERS MEGA CONTENT -->
              <div v-if="link.label === 'PROVIDERS'" class="mega-menu__inner">
                <!-- LEFT: Text panel -->
                <div class="mega-menu__left">
                  <h2 class="mega-menu__heading">Provider Services</h2>
                  <p class="mega-menu__subtext">
                    Find trusted compounded pharmacy providers who deliver personalized medications that make a difference.
                  </p>
                  <div class="mega-menu__buttons">
                    <a href="#become-provider" class="mega-btn">
                      Become a Provider
                      <ArrowUpRight :size="16" />
                    </a>
                    <a href="#provider-resources" class="mega-btn">
                      Provider Resources
                      <ArrowUpRight :size="16" />
                    </a>
                    <a href="#prescribe" class="mega-btn">
                      Prescribe with Revita (Network Join)
                      <ArrowUpRight :size="16" />
                    </a>
                  </div>
                </div>

                <!-- RIGHT: Images panel -->
                <div class="mega-menu__right-container">
                  <h3 class="mega-menu__right-heading">Our Services</h3>
                  <div class="mega-menu__right">
                    <!-- Big image: Doctor -->
                    <div class="mega-img mega-img--big">
                      <img src="/doctor.png" alt="Doctors" />
                      <div class="mega-img__overlay">
                        <span class="mega-img__title">Doctors</span>
                        <span class="mega-img__arrow"><ArrowUpRight :size="18" /></span>
                      </div>
                    </div>

                    <!-- Two small images stacked -->
                    <div class="mega-img-stack">
                      <div class="mega-img mega-img--small mega-img--top">
                        <img src="/hospital.png" alt="Hospitals" />
                        <div class="mega-img__overlay">
                          <span class="mega-img__title">Hospitals</span>
                          <span class="mega-img__arrow"><ArrowUpRight :size="16" /></span>
                        </div>
                      </div>
                      <div class="mega-img mega-img--small mega-img--bottom">
                        <img src="/clinic.png" alt="Clinics" />
                        <div class="mega-img__overlay">
                          <span class="mega-img__title">Clinics</span>
                          <span class="mega-img__arrow"><ArrowUpRight :size="16" /></span>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- PATIENTS MEGA CONTENT -->
              <div v-if="link.label === 'PATIENTS'" class="mega-menu__inner">
                <!-- LEFT: Text panel -->
                <div class="mega-menu__left mega-menu__left--large">
                  <h2 class="mega-menu__heading">Patient Services</h2>
                  <p class="mega-menu__subtext">
                    Explore patient services designed to support your care and personalized medication needs.
                  </p>
                </div>

                <!-- RIGHT: Images panel -->
                <div class="mega-menu__right-container">
                  <h3 class="mega-menu__right-heading">Patients Services</h3>
                  <div class="mega-menu__right">
                    <!-- Big image: Upload Prescription -->
                    <div class="mega-img mega-img--big">
                      <img src="/upload-prescription.png" alt="Upload Prescription" />
                      <div class="mega-img__overlay">
                        <span class="mega-img__title">Upload Prescription</span>
                        <span class="mega-img__arrow"><ArrowUpRight :size="18" /></span>
                      </div>
                    </div>

                    <!-- Two small images stacked -->
                    <div class="mega-img-stack">
                      <div class="mega-img mega-img--small mega-img--top">
                        <img src="/find-provider.png" alt="Find a Provider" />
                        <div class="mega-img__overlay">
                          <span class="mega-img__title">Find a Provider</span>
                          <span class="mega-img__arrow"><ArrowUpRight :size="16" /></span>
                        </div>
                      </div>
                      <div class="mega-img mega-img--small mega-img--bottom">
                        <img src="/patient-resources.png" alt="Patient Resources & Guides" />
                        <div class="mega-img__overlay">
                          <span class="mega-img__title">Patient Resources & Guides</span>
                          <span class="mega-img__arrow"><ArrowUpRight :size="16" /></span>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              
              <!-- COMPOUNDED PREPARATIONS MEGA CONTENT -->
              <div v-if="link.label === 'COMPOUNDED PREPARATIONS'" class="mega-menu__inner">
                <!-- LEFT: Search and Categories -->
                <div class="mega-menu__left">
                  <h2 class="mega-menu__heading mega-menu__heading--small">Find compounded medications and other products that make a difference</h2>
                  
                  <div class="mega-search">
                    <Search :size="18" class="mega-search__icon" />
                    <input type="text" placeholder="Search products..." class="mega-search__input" />
                  </div>

                  <div class="mega-categories">
                    <h4 class="mega-categories__title">Categories</h4>
                    <ul class="mega-categories__list">
                      <li><a href="#cat-hormone">Hormone Health</a></li>
                      <li><a href="#cat-weight">Weight Management</a></li>
                      <li><a href="#cat-skin">Skin Care</a></li>
                      <li><a href="#cat-vitamins">Vitamins & Supplements</a></li>
                      <li><a href="#cat-pediatric">Pediatric Compounding</a></li>
                    </ul>
                  </div>
                </div>

                <!-- RIGHT: Product Cards -->
                <div class="mega-menu__right-container">
                  <h3 class="mega-menu__right-heading">Featured Products</h3>
                  <div class="mega-products">
                    <div class="product-card">
                      <div class="product-card__img">
                        <img src="/WhatsApp Image 2026-05-01 at 12.23.06 AM (5).jpeg" alt="Pregnenolone Capsules" />
                      </div>
                      <h4 class="product-card__title">Pregnenolone Capsules</h4>
                    </div>

                    <div class="product-card">
                      <div class="product-card__img">
                        <img src="/WhatsApp Image 2026-05-01 at 12.23.06 AM (6).jpeg" alt="DHEA Capsule" />
                      </div>
                      <h4 class="product-card__title">DHEA Capsule</h4>
                    </div>

                    <div class="product-card">
                      <div class="product-card__img">
                        <img src="/WhatsApp Image 2026-05-01 at 12.23.06 AM (7).jpeg" alt="Liothyronine Sodium / Levothyroxine Sodium" />
                      </div>
                      <h4 class="product-card__title">Liothyronine Sodium / Levothyroxine Sodium</h4>
                    </div>
                  </div>
                </div>
              </div>

              <!-- SUPPORT MEGA CONTENT -->
              <div v-if="link.label === 'SUPPORT'" class="mega-menu__inner">
                <div class="mega-menu__left">
                  <h2 class="mega-menu__heading">We're Here to Help</h2>
                  <p class="mega-menu__subtext">
                    Get the support you need, whether you're looking for a location or need to speak with our experts.
                  </p>
                  <div class="mega-menu__buttons">
                    <a href="#contact" class="mega-btn">
                      Contact Us
                      <ArrowUpRight :size="16" />
                    </a>
                    <a href="#locations" class="mega-btn">
                      Revitalife Locations
                      <ArrowUpRight :size="16" />
                    </a>
                  </div>
                </div>
                <div class="mega-menu__right-container">
                  <h3 class="mega-menu__right-heading">Support Channels</h3>
                  <div class="mega-menu__right">
                    <div class="mega-img mega-img--big">
                      <img src="/contact_image.png" alt="Contact Support" />
                      <div class="mega-img__overlay">
                        <span class="mega-img__title">Contact Us</span>
                        <span class="mega-img__arrow"><ArrowUpRight :size="18" /></span>
                      </div>
                    </div>
                    <div class="mega-img">
                      <img src="/location_image.png" alt="Our Locations" />
                      <div class="mega-img__overlay">
                        <span class="mega-img__title">Locations</span>
                        <span class="mega-img__arrow"><ArrowUpRight :size="18" /></span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- RESOURCES MEGA CONTENT -->
              <div v-if="link.label === 'RESOURCES'" class="mega-menu__inner">
                <div class="mega-menu__left">
                  <h2 class="mega-menu__heading">Knowledge Center</h2>
                  <p class="mega-menu__subtext">
                    Explore our latest insights, health guides, and frequently asked questions.
                  </p>
                  <div class="mega-menu__buttons">
                    <a href="#faq" class="mega-btn">
                      Frequently Asked Questions
                      <ArrowUpRight :size="16" />
                    </a>
                    <a href="#blog" class="mega-btn">
                      Health Blog & News
                      <ArrowUpRight :size="16" />
                    </a>
                  </div>
                </div>
                <div class="mega-menu__right-container">
                  <h3 class="mega-menu__right-heading">Learn More</h3>
                  <div class="mega-menu__right">
                    <div class="mega-img mega-img--big">
                      <img src="/faq_image.png" alt="FAQ" />
                      <div class="mega-img__overlay">
                        <span class="mega-img__title">FAQ</span>
                        <span class="mega-img__arrow"><ArrowUpRight :size="18" /></span>
                      </div>
                    </div>
                    <div class="mega-img">
                      <img src="/blogs_image.png" alt="Health Blogs" />
                      <div class="mega-img__overlay">
                        <span class="mega-img__title">Blogs</span>
                        <span class="mega-img__arrow"><ArrowUpRight :size="18" /></span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- COMPANY MEGA CONTENT -->
              <div v-if="link.label === 'COMPANY'" class="mega-menu__inner">
                <div class="mega-menu__left">
                  <h2 class="mega-menu__heading">Our Mission</h2>
                  <p class="mega-menu__subtext">
                    Dedicated to providing the highest quality compounded medications and personalized care.
                  </p>
                  <div class="mega-menu__buttons">
                    <a href="#team" class="mega-btn">
                      Meet Our Team
                      <ArrowUpRight :size="16" />
                    </a>
                    <a href="#careers" class="mega-btn">
                      Join the Revitalife Family
                      <ArrowUpRight :size="16" />
                    </a>
                  </div>
                </div>
                <div class="mega-menu__right-container">
                  <h3 class="mega-menu__right-heading">Who We Are</h3>
                  <div class="mega-menu__right">
                    <div class="mega-img mega-img--big">
                      <img src="/team_image.png" alt="Our Team" />
                      <div class="mega-img__overlay">
                        <span class="mega-img__title">Our Team</span>
                        <span class="mega-img__arrow"><ArrowUpRight :size="18" /></span>
                      </div>
                    </div>
                    <div class="mega-img">
                      <img src="/careers_image.png" alt="Careers" />
                      <div class="mega-img__overlay">
                        <span class="mega-img__title">Careers</span>
                        <span class="mega-img__arrow"><ArrowUpRight :size="18" /></span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </li>
        </ul>

        <!-- Desktop Actions -->
        <div class="navbar__actions">
          <a href="#find-provider" class="navbar__link-action">FIND A PROVIDER</a>
          <a href="#become-provider" class="btn btn-primary btn-pill">BECOME A PROVIDER</a>
        </div>

        <!-- Mobile Toggle -->
        <button
          :class="['navbar__hamburger', { open: menuOpen }]"
          @click="menuOpen = !menuOpen"
          :aria-label="menuOpen ? 'Close menu' : 'Open menu'"
        >
          <span></span><span></span><span></span>
        </button>
      </div>

      <!-- Mobile Menu -->
      <div :class="['navbar__mobile-menu', { open: menuOpen }]" :aria-hidden="!menuOpen">
        <div class="navbar__mobile-inner">
          <div v-for="link in NAV_LINKS" :key="link.label" class="navbar__mobile-item">
            <div 
              class="navbar__mobile-link-row" 
              @click="handleLinkClick(link)"
            >
              <a :href="link.href" class="navbar__mobile-link" @click.stop="!link.hasMega && handleLinkClick(link)">
                {{ link.label }}
              </a>
              <button v-if="link.hasMega" class="navbar__mobile-toggle">
                <span class="navbar__link-icon">{{ activeMobileMega === link.label ? '−' : '+' }}</span>
              </button>
            </div>

            <!-- Mobile Mega Content -->
            <div 
              v-if="link.hasMega" 
              :class="['navbar__mobile-mega', { open: activeMobileMega === link.label }]"
            >
              <!-- Providers content simplified -->
              <div v-if="link.label === 'PROVIDERS'" class="mobile-mega-list">
                <a href="#become-provider" @click="menuOpen = false">Become a Provider</a>
                <a href="#provider-resources" @click="menuOpen = false">Provider Resources</a>
                <a href="#prescribe" @click="menuOpen = false">Prescribe with Revita</a>
              </div>

              <!-- Patients content simplified -->
              <div v-if="link.label === 'PATIENTS'" class="mobile-mega-list">
                <p class="mobile-mega-desc">Explore patient services designed to support your care and personalized medication needs.</p>
                <a href="#upload" @click="menuOpen = false">Upload Prescription</a>
                <a href="#find" @click="menuOpen = false">Find a Provider</a>
                <a href="#resources" @click="menuOpen = false">Patient Resources & Guides</a>
              </div>

              <!-- Preparations content simplified -->
              <div v-if="link.label === 'COMPOUNDED PREPARATIONS'" class="mobile-mega-list">
                <div class="mobile-mega-search">
                  <input type="text" placeholder="Search products..." />
                </div>
                <div class="mobile-mega-sublist">
                  <h5 class="mobile-mega-title">Featured Products</h5>
                  <a href="#p1" @click="menuOpen = false">Pregnenolone Capsules</a>
                  <a href="#p2" @click="menuOpen = false">DHEA Capsule</a>
                  <a href="#p3" @click="menuOpen = false">Liothyronine / Levothyroxine</a>
                </div>
              </div>

              <!-- Support content simplified -->
              <div v-if="link.label === 'SUPPORT'" class="mobile-mega-list">
                <p class="mobile-mega-desc">We're here to help you find what you need.</p>
                <a href="#contact" @click="menuOpen = false">Contact Us</a>
                <a href="#locations" @click="menuOpen = false">Revitalife Locations</a>
              </div>

              <!-- Resources content simplified -->
              <div v-if="link.label === 'RESOURCES'" class="mobile-mega-list">
                <p class="mobile-mega-desc">Insights and guides for your health journey.</p>
                <a href="#faq" @click="menuOpen = false">Frequently Asked Questions</a>
                <a href="#blog" @click="menuOpen = false">Health Blog & News</a>
              </div>

              <!-- Company content simplified -->
              <div v-if="link.label === 'COMPANY'" class="mobile-mega-list">
                <p class="mobile-mega-desc">Committed to quality and personalized care.</p>
                <a href="#team" @click="menuOpen = false">Meet Our Team</a>
                <a href="#careers" @click="menuOpen = false">Join our Family (Careers)</a>
              </div>
            </div>
          </div>
          
          <div class="navbar__mobile-actions">
            <a href="#find-provider" class="btn btn-outline" @click="menuOpen = false">FIND A PROVIDER</a>
            <a href="#become-provider" class="btn btn-primary" @click="menuOpen = false">BECOME A PROVIDER</a>
          </div>
        </div>
      </div>
    </nav>

    <SearchModal :isOpen="isSearchOpen" @close="isSearchOpen = false" />
  </header>
</template>
