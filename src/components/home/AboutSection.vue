<script setup lang="ts"></script>

<template>
  <section id="about" class="about section-shell">
    <div class="container">
      <div class="section-title">
        <h2>About Us</h2>
        <div class="title-line"></div>
      </div>
      <p class="about-copy">
        Welcome to Joson-Perey Dental Clinic, where your smile is our priority. Located in Mendez,
        Cavite, our experienced dentists are committed to providing top-quality dental care in a
        warm and welcoming environment. From routine check-ups to specialized treatments, we offer
        comprehensive dental services to ensure your oral health and well-being. Come experience
        the difference at Joson-Perey Dental Clinic – your trusted partner in oral care.
      </p>

      <div class="about-middle">
        <article class="info-card">
          <h3>Our Mission</h3>
          <p>
            To provide exceptional dental care, promote oral health, and enhance the well-being of
            our patients.
          </p>
        </article>

        <article class="info-card">
          <h3>Our Vision</h3>
          <p>
            To be a trusted leader in dental excellence, continually innovating and setting new
            standards in patient-centered care.
          </p>
        </article>
      </div>

      <div class="stats">
        <article class="stat-card">
          <div class="stat-icon">◷</div>
          <h3>Years of Experience</h3>
          <p>Over 45 years of providing exceptional healthcare services.</p>
        </article>

        <article class="stat-card">
          <div class="stat-icon">❤</div>
          <h3>Total Satisfied Patients</h3>
          <p>More than 80,000 patients served and satisfied.</p>
        </article>

        <article class="stat-card">
          <div class="stat-icon">⚙</div>
          <h3>Advanced Technology</h3>
          <p>Utilizing cutting-edge technology for better patient care.</p>
        </article>
      </div>
    </div>
  </section>
</template>

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

.about {
  background: #fff;
}

.about-copy {
  max-width: 980px;
  margin: 0 auto 36px;
  color: #42576a;
  text-align: center;
  font-size: 1.08rem;
}

.about-middle {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 24px;
  max-width: 860px;
  margin: 0 auto 34px;
}

.info-card {
  padding: 26px;
  border: 1px solid #d7e3ea;
  border-radius: 18px;
  background: #fff;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
}

.info-card h3 {
  margin: 0 0 12px;
  color: #1e3342;
  font-size: 1.5rem;
}

.info-card p {
  margin: 0;
  color: #435767;
  font-size: 1.03rem;
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 18px;
  max-width: 980px;
  margin: 0 auto;
}

.stat-card {
  padding: 26px 20px;
  border-radius: 16px;
  background: linear-gradient(135deg, #0a335c, #0b4f7b 55%, #0d6999);
  color: #fff;
  text-align: center;
  box-shadow: 0 8px 20px rgba(8, 60, 109, 0.28);
}

.stat-icon {
  display: grid;
  place-items: center;
  width: 54px;
  height: 54px;
  margin: 0 auto 14px;
  border: 1px solid rgba(255, 255, 255, 0.35);
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.18);
  font-size: 1.6rem;
  font-weight: 700;
}

.stat-card h3 {
  margin: 0 0 8px;
  font-size: 1.65rem;
  line-height: 1.15;
}

.stat-card p {
  margin: 0;
  color: rgba(255, 255, 255, 0.96);
  font-size: 1rem;
}

@media (max-width: 1080px) {
  .stats {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 820px) {
  .about-middle {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 640px) {
  .section-shell {
    padding: 70px 0;
  }

  .stats {
    grid-template-columns: 1fr;
  }
}
</style>
