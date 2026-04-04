<template>
  <section id="services" class="services section-shell">
    <div class="container">
      <div class="section-title">
        <h2>Our Services</h2>
        <div class="title-line"></div>
      </div>
      <p class="section-intro">
        Welcome to our services section. We offer a wide range of services to meet your needs.
        Our team is dedicated to providing high-quality solutions to our clients. Explore our
        services below to learn more about what we can offer you.
      </p>

      <div class="services-grid">
        <article v-for="service in services" :key="service.title" class="service-card">
          <img :src="service.image" :alt="service.title" />
          <div class="service-body">
            <h3>{{ service.title }}</h3>
            <p>{{ service.description }}</p>
          </div>
        </article>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import oralProphylaxis from '@/assets/images/services/oral-prophylaxis.webp'
import extraction from '@/assets/images/services/extraction.webp'
import fillings from '@/assets/images/services/fillings.webp'
import fluorideApplication from '@/assets/images/services/fluoride-application.webp'
import periodontalTreatment from '@/assets/images/services/periodontal-treatment.webp'
import endodonticTreatment from '@/assets/images/services/endodontic-treatment.webp'
import orthodonticTreatment from '@/assets/images/services/orthodontic-treatment.webp'
import prosthodonticTreatment from '@/assets/images/services/prosthodontic-treatment.webp'
import cosmeticDentistry from '@/assets/images/services/cosmetic-dentistry.webp'

interface ServiceItem {
  title: string
  description: string
  image: string
}

const services: ServiceItem[] = [
  {
    title: 'Oral Prophylaxis',
    description:
      'A dental cleaning procedure to remove plaque and tartar, preventing tooth and gum diseases.',
    image: oralProphylaxis,
  },
  {
    title: 'Extraction',
    description: 'The removal of a damaged or problematic tooth from the mouth.',
    image: extraction,
  },
  {
    title: 'Fillings',
    description: 'Restoring tooth structure by filling cavities with dental materials.',
    image: fillings,
  },
  {
    title: 'Fluoride Application',
    description: 'The application of fluoride to strengthen tooth enamel and prevent decay.',
    image: fluorideApplication,
  },
  {
    title: 'Periodontal Treatment',
    description: 'Procedures to treat gum diseases and maintain healthy gums.',
    image: periodontalTreatment,
  },
  {
    title: 'Endodontic Treatment',
    description: 'Root canal therapy to save a diseased or infected tooth.',
    image: endodonticTreatment,
  },
  {
    title: 'Orthodontic Treatment',
    description: 'The correction of misaligned teeth and jaws using braces or aligners.',
    image: orthodonticTreatment,
  },
  {
    title: 'Prosthodontic Treatment',
    description:
      'Restoration and replacement of missing teeth with dentures, bridges, or dental implants.',
    image: prosthodonticTreatment,
  },
  {
    title: 'Cosmetic Dentistry',
    description:
      'Enhancing the appearance of teeth through procedures like teeth whitening, veneers, and dental bonding.',
    image: cosmeticDentistry,
  },
]
</script>

<style scoped>
.section-shell {
  padding: 86px 0;
}

.section-title {
  margin-bottom: 20px;
  text-align: center;
}

.section-title h2 {
  margin: 0;
  color: var(--navy);
  font-size: clamp(1.9rem, 3vw, 2.7rem);
  font-weight: 800;
  letter-spacing: -0.03em;
  text-transform: uppercase;
}

.title-line {
  width: 72px;
  height: 3px;
  margin: 14px auto 0;
  border-radius: 99px;
  background: var(--navy);
}

.section-intro {
  max-width: 1020px;
  margin: 0 auto 34px;
  color: #2d4961;
  text-align: center;
  font-size: 1.08rem;
}

.services {
  background: #fff;
}

.services-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 18px;
}

.service-card {
  overflow: hidden;
  border: 1px solid var(--line);
  border-radius: 18px;
  background: var(--card);
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.06);
}

.service-card img {
  width: 100%;
  height: 178px;
  object-fit: cover;
}

.service-body {
  padding: 22px 22px 24px;
  text-align: center;
}

.service-body h3 {
  margin: 0 0 10px;
  color: var(--navy);
  font-size: 1.35rem;
  line-height: 1.2;
}

.service-body p {
  margin: 0;
  color: #243947;
  font-size: 1rem;
}

@media (max-width: 1080px) {
  .services-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 640px) {
  .section-shell {
    padding: 70px 0;
  }

  .services-grid {
    grid-template-columns: 1fr;
  }
}
</style>
