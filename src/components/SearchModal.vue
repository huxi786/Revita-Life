<script setup>
import { ref, watch, nextTick } from 'vue';
import { Search, X } from 'lucide-vue-next';
import '../styles/SearchModal.css';

const props = defineProps({
  isOpen: Boolean
});

const emit = defineEmits(['close']);

const searchQuery = ref('');
const searchInput = ref(null);

// Comprehensive Full-Site Index (Simulating a real site crawler)
const SEARCHABLE_CONTENT = [
  // PRODUCTS & PREPARATIONS
  {
    id: 'p1',
    title: 'Pregnenolone & Hormone Health',
    snippet: 'Customized hormone replacement therapies tailored to your needs.',
    url: 'WWW.REVITALIFE.COM/SERVICES/HORMONE-HEALTH',
    keywords: ['pregnenolone', 'hormone', 'health', 'hrt', 'replacement', 'therapy'],
    targetId: '#service-hormone-replacement'
  },
  {
    id: 'p2',
    title: 'Dermatology & Skin Care',
    snippet: 'Professional azelaic, tranexamic, and custom skin formulations.',
    url: 'WWW.REVITALIFE.COM/SERVICES/DERMATOLOGY',
    keywords: ['dermatology', 'skin', 'cream', 'acne', 'anti-aging'],
    targetId: '#service-dermatology'
  },
  {
    id: 'p5',
    title: 'Weight Management',
    snippet: 'Personalized weight loss medications and metabolic support.',
    url: 'WWW.REVITALIFE.COM/SERVICES/WEIGHT-MANAGEMENT',
    keywords: ['weight', 'management', 'loss', 'metabolism', 'diet', 'obesity'],
    targetId: '#service-weight-management'
  },
  
  // FAQs
  {
    id: 'r1',
    title: 'What is compounding?',
    snippet: 'Learn about the art and science of customized medications.',
    url: 'WWW.REVITALIFE.COM/RESOURCES/FAQ',
    keywords: ['compounding', 'what', 'is', 'faq', 'custom'],
    targetId: '#faq-1'
  },
  {
    id: 'r3',
    title: 'Do I need a prescription?',
    snippet: 'Valid prescriptions are required for all compounded medications.',
    url: 'WWW.REVITALIFE.COM/RESOURCES/FAQ',
    keywords: ['prescription', 'need', 'doctor', 'rx', 'order'],
    targetId: '#faq-2'
  },
  {
    id: 'pr1',
    title: 'Become a Provider',
    snippet: 'Join our network of healthcare professionals and prescribe personalized medication.',
    url: 'WWW.REVITALIFE.COM/PROVIDERS/BECOME-A-PARTNER',
    keywords: ['become', 'provider', 'partner', 'doctor', 'clinic', 'prescribe'],
    targetId: '#providers'
  },
  {
    id: 'pr2',
    title: 'Provider Resources',
    snippet: 'Access clinical data, formulary guides, and pharmacy consultation services.',
    url: 'WWW.REVITALIFE.COM/PROVIDERS/RESOURCES',
    keywords: ['provider', 'resources', 'data', 'clinical', 'formulary', 'guide'],
    targetId: '#providers'
  },
  {
    id: 'pr3',
    title: 'Prescribe with Revitalife',
    snippet: 'How to easily send prescriptions to our compounding laboratory.',
    url: 'WWW.REVITALIFE.COM/PROVIDERS/PRESCRIBE',
    keywords: ['prescribe', 'order', 'send', 'prescription', 'lab'],
    targetId: '#providers'
  },

  // PATIENTS
  {
    id: 'pa1',
    title: 'Upload Prescription',
    snippet: 'Quickly and securely upload your prescription for fulfillment.',
    url: 'WWW.REVITALIFE.COM/PATIENTS/UPLOAD',
    keywords: ['upload', 'prescription', 'send', 'rx', 'order'],
    targetId: '#patients'
  },
  {
    id: 'pa2',
    title: 'Find a Provider',
    snippet: 'Locate a medical professional in our network who understands compounding.',
    url: 'WWW.REVITALIFE.COM/PATIENTS/FIND-A-DOCTOR',
    keywords: ['find', 'provider', 'doctor', 'specialist', 'clinic'],
    targetId: '#patients'
  },
  {
    id: 'pa3',
    title: 'Patient Resources & Guides',
    snippet: 'Education on how to use your medications and manage your health.',
    url: 'WWW.REVITALIFE.COM/PATIENTS/RESOURCES',
    keywords: ['patient', 'resources', 'guides', 'education', 'learning'],
    targetId: '#patients'
  },

  // SUPPORT & LOCATIONS
  {
    id: 's1',
    title: 'Contact Support',
    snippet: 'Speak with our pharmacy experts for help with your medications.',
    url: 'WWW.REVITALIFE.COM/SUPPORT/CONTACT',
    keywords: ['contact', 'support', 'help', 'phone', 'email', 'chat'],
    targetId: '#support'
  },
  {
    id: 's2',
    title: 'Revitalife Pharmacy Locations',
    snippet: 'Visit one of our state-of-the-art pharmacy facilities near you.',
    url: 'WWW.REVITALIFE.COM/SUPPORT/LOCATIONS',
    keywords: ['locations', 'address', 'near', 'me', 'pharmacy', 'branch'],
    targetId: '#support'
  },

  // RESOURCES
  {
    id: 'r1',
    title: 'Frequently Asked Questions',
    snippet: 'Answers to common questions about compounding and shipping.',
    url: 'WWW.REVITALIFE.COM/RESOURCES/FAQ',
    keywords: ['faq', 'questions', 'answers', 'shipping', 'cost', 'compounding'],
    targetId: '#resources'
  },
  {
    id: 'r2',
    title: 'Health Blog & News',
    snippet: 'Latest insights on personalized medicine and wellness.',
    url: 'WWW.REVITALIFE.COM/RESOURCES/BLOG',
    keywords: ['blog', 'news', 'articles', 'health', 'wellness', 'updates'],
    targetId: '#resources'
  },

  // COMPANY
  {
    id: 'c1',
    title: 'Meet Our Team',
    snippet: 'Learn about the pharmacists and experts behind Revitalife.',
    url: 'WWW.REVITALIFE.COM/COMPANY/TEAM',
    keywords: ['team', 'staff', 'pharmacists', 'experts', 'leadership'],
    targetId: '#company'
  },
  {
    id: 'c2',
    title: 'Careers at Revitalife',
    snippet: 'Join our growing team of dedicated healthcare professionals.',
    url: 'WWW.REVITALIFE.COM/COMPANY/CAREERS',
    keywords: ['careers', 'jobs', 'hiring', 'work', 'employment'],
    targetId: '#company'
  }
];

const results = ref([]);

watch(() => props.isOpen, (newVal) => {
  if (newVal) {
    document.body.style.overflow = 'hidden';
    nextTick(() => {
      searchInput.value?.focus();
    });
  } else {
    document.body.style.overflow = '';
    searchQuery.value = '';
    results.value = [];
  }
});

watch(searchQuery, (newQuery) => {
  const q = newQuery.trim().toLowerCase();
  if (!q) {
    results.value = [];
    return;
  }
  
  // Search in title, snippet, and keywords
  results.value = SEARCHABLE_CONTENT.filter(item => 
    item.title.toLowerCase().includes(q) || 
    item.snippet.toLowerCase().includes(q) ||
    item.keywords.some(k => k.toLowerCase().includes(q))
  );
});

const clearSearch = () => {
  searchQuery.value = '';
  results.value = [];
  searchInput.value?.focus();
};

const handleResultClick = (targetId) => {
  // 1. First, close the search modal
  emit('close');
  
  // 2. Wait for the slide-up animation to complete (0.6s as defined in CSS)
  // before attempting to scroll, to ensure correct layout calculations.
  setTimeout(() => {
    const element = document.querySelector(targetId);
    if (element) {
      const headerOffset = 100; // Adjust based on your sticky navbar height
      const elementPosition = element.getBoundingClientRect().top;
      const offsetPosition = elementPosition + window.pageYOffset - headerOffset;

      window.scrollTo({
        top: offsetPosition,
        behavior: 'smooth'
      });
      
      // 3. Add a temporary highlight effect to the target section
      element.classList.add('section-highlight');
      setTimeout(() => {
        element.classList.remove('section-highlight');
      }, 2000);
    }
  }, 500); 
};
</script>

<template>
  <div class="search-wrapper">
    <!-- Dark Background Overlay -->
    <div :class="['search-overlay', { open: isOpen }]" @click="emit('close')"></div>
    
    <!-- White Slide-down Modal -->
    <div :class="['search-modal', { open: isOpen }]">
      <div class="search-modal__container">
        
        <button class="search-modal__close" @click="emit('close')" aria-label="Close search">
          <X :size="32" stroke-width="1.5" />
        </button>

        <div class="search-modal__header">
          <button v-if="searchQuery" class="search-modal__clear" @click="clearSearch">
            CLEAR SEARCH
          </button>
        </div>

        <div :class="['search-modal__input-wrapper', { 'has-results': results.length > 0 || !searchQuery }]">
          <input 
            ref="searchInput"
            type="text" 
            v-model="searchQuery"
            class="search-modal__input" 
            placeholder="Search Products and More..."
          />
          <Search class="search-modal__icon" :size="28" stroke-width="1.5" />
        </div>

        <!-- Professional Suggestions (When Search is empty) -->
        <div v-if="!searchQuery" class="search-modal__suggestions">
          <h3 class="search-modal__results-title">Popular Suggestions</h3>
          <div class="search-modal__tags">
            <button class="search-modal__tag" @click="searchQuery = 'Hormone'">Hormone Health</button>
            <button class="search-modal__tag" @click="searchQuery = 'Weight'">Weight Loss</button>
            <button class="search-modal__tag" @click="searchQuery = 'Provider'">Provider Login</button>
            <button class="search-modal__tag" @click="searchQuery = 'FAQ'">How to order?</button>
          </div>
        </div>

        <!-- Search Results List -->
        <div v-if="searchQuery && results.length > 0" class="search-modal__results">
          <div 
            v-for="item in results" 
            :key="item.id" 
            class="search-result-item"
            @click="handleResultClick(item.targetId)"
          >
            <h4 class="search-result-title">{{ item.title }}</h4>
            <p class="search-result-snippet" v-html="item.snippet"></p>
            <span class="search-result-url">{{ item.url }}</span>
          </div>
        </div>

        <!-- No Results -->
        <div v-if="searchQuery && results.length === 0" class="search-modal__empty">
          <p>No results found for "<strong>{{ searchQuery }}</strong>"</p>
          <span>Try checking for typos or use more general terms like "Medication" or "Support".</span>
        </div>

      </div>
    </div>
  </div>
</template>
