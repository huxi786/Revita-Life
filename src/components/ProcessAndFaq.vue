<script setup>
import { ref } from 'vue';
import { ChevronDown, MessageSquare, ClipboardCheck, FlaskConical, Truck } from 'lucide-vue-next';
import '../styles/ProcessFaq.css';

const STEPS = [
  {
    icon: MessageSquare,
    title: '1. Consultation',
    desc: 'Your provider sends us your prescription, or you can consult with our clinical pharmacists directly.'
  },
  {
    icon: ClipboardCheck,
    title: '2. Review & Setup',
    desc: 'We verify the formula, check for allergies, and securely process your payment.'
  },
  {
    icon: FlaskConical,
    title: '3. Compounding',
    desc: 'Our specialized technicians compound your medication in our state-of-the-art cleanrooms.'
  },
  {
    icon: Truck,
    title: '4. Delivery',
    desc: 'Your custom medication is shipped securely to your door or ready for local pickup.'
  }
];

const FAQS = [
  {
    q: 'What is compounding?',
    a: 'Pharmacy compounding is the art and science of preparing customized medications for patients. Its origin can be traced back to the origins of pharmacy. It was the standard method of providing prescription medications before drugs began to be produced in mass quantities by pharmaceutical manufacturers.'
  },
  {
    q: 'Do I need a prescription?',
    a: 'Yes. Compounded medications require a valid prescription from a licensed healthcare provider, just like any other prescription drug.'
  },
  {
    q: 'Is compounding safe and approved?',
    a: 'While compounded medications themselves are not FDA-approved, the ingredients used are. We source all our active pharmaceutical ingredients (APIs) from FDA-registered facilities and adhere strictly to USP guidelines.'
  },
  {
    q: 'Does insurance cover compounded medications?',
    a: 'Coverage varies significantly by insurance plan. While we do not process insurance directly, we can provide you with a universal claim form to submit to your provider for potential reimbursement.'
  }
];

const openFaqIndex = ref(0);

const toggleFaq = (index) => {
  openFaqIndex.value = openFaqIndex.value === index ? null : index;
};
</script>

<template>
  <div>
    <!-- How it Works Section -->
    <section class="process section-padding" id="process">
      <div class="container">
        <div class="process__header" v-motion-slide-visible-once-bottom>
          <span class="section-label">The Process</span>
          <h2 class="process__title">How Compounding <span class="text-gradient">Works</span></h2>
        </div>

        <div class="process__grid">
          <div 
            v-for="(step, i) in STEPS" 
            :key="i"
            :id="'step-' + (i+1)"
            class="process-card glass"
            v-motion
            :initial="{ opacity: 0, y: 30 }"
            :enter="{ opacity: 1, y: 0, transition: { delay: i * 150 } }"
          >
            <div class="process-card__icon-wrapper">
              <component :is="step.icon" :size="28" class="process-card__icon" />
            </div>
            <h3 class="process-card__title">{{ step.title }}</h3>
            <p class="process-card__desc">{{ step.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- FAQ Section -->
    <section class="faq section-padding-lg" id="faq">
      <div class="container faq__container">
        <div class="faq__content" v-motion-slide-visible-once-left>
          <span class="section-label">Questions?</span>
          <h2 class="faq__title">Frequently Asked <span class="text-gradient-gold">Questions</span></h2>
          <p class="faq__desc">
            Compounding can seem complex. We're here to make it simple. 
            If you don't see your question answered here, don't hesitate to reach out to our pharmacists.
          </p>
          <a href="#contact" class="btn btn-outline-white">Ask a Pharmacist</a>
        </div>

        <div class="faq__accordion" v-motion-slide-visible-once-right>
          <div 
            v-for="(faq, i) in FAQS" 
            :key="i"
            :id="'faq-' + (i+1)"
            :class="['faq__item', { open: openFaqIndex === i }]"
          >
            <button 
              class="faq__question" 
              @click="toggleFaq(i)"
              :aria-expanded="openFaqIndex === i"
              :aria-controls="`faq-answer-${i}`"
            >
              {{ faq.q }}
              <ChevronDown 
                class="faq__icon" 
                :size="20" 
              />
            </button>
            <div 
              :id="`faq-answer-${i}`" 
              class="faq__answer-wrapper"
              :aria-hidden="openFaqIndex !== i"
            >
              <div class="faq__answer">
                {{ faq.a }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
