<template>
  <section id="doctors" class="doctors section-shell">
    <div class="container">
      <div class="section-title">
        <h2>Our Doctors</h2>
        <div class="title-line"></div>
      </div>
      <p class="section-intro">
        Our Dental Clinic is committed to providing the best dental services. We have a team of
        experienced and caring professionals to take care of your dental needs.
      </p>

      <div class="doctors-grid">
        <article v-for="doctor in doctors" :key="doctor.name" class="doctor-card">
          <div class="doctor-photo">
            <img :src="doctor.image" :alt="doctor.name" />
          </div>
          <h3>{{ doctor.name }}</h3>
          <p>{{ doctor.description }}</p>
        </article>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import divinaPhoto from '@/assets/images/doctors/divina.webp'
import rachellePhoto from '@/assets/images/doctors/rachelle.webp'

interface DoctorItem {
  name: string
  description: string
  image: string
}

const doctors: DoctorItem[] = [
  {
    name: 'Dr. Divina P. Joson',
    description:
      'Dr. Divina P. Joson is an experienced dentist with a passion for oral health. She has been serving patients for over 10 years and is known for her gentle approach and dedication to patient care.',
    image: divinaPhoto,
  },
  {
    name: 'Dr. Rachelle May P. Joson',
    description:
      'Dr. Rachelle May P. Joson is a skilled dentist who believes in creating beautiful smiles. With her expertise in cosmetic dentistry, she has transformed the lives of many patients.',
    image: rachellePhoto,
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

.doctors {
  background: var(--sky-soft);
}

.doctors-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 30px;
  max-width: 980px;
  margin: 0 auto;
}

.doctor-card {
  padding: 12px 18px;
  text-align: center;
}

.doctor-photo {
  overflow: hidden;
  width: 220px;
  height: 280px;
  margin: 0 auto 18px;
  border: 5px solid #fff;
  border-radius: 24px;
  background: #c6ddec;
  box-shadow: 0 8px 18px rgba(15, 96, 148, 0.2);
}

.doctor-photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.doctor-card h3 {
  margin: 0 0 10px;
  color: #222;
  font-size: 1.75rem;
  line-height: 1.2;
}

.doctor-card p {
  max-width: 420px;
  margin: 0 auto;
  color: #263a47;
  font-size: 1.03rem;
}

@media (max-width: 820px) {
  .doctors-grid {
    grid-template-columns: 1fr;
  }

  .doctor-card h3 {
    font-size: 1.5rem;
  }
}

@media (max-width: 640px) {
  .section-shell {
    padding: 70px 0;
  }
}
</style>
