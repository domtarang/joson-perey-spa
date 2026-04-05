<script setup lang="ts"></script>

<template>
  <section id="contact" class="contact section-shell">
    <div class="container">
      <div class="section-title">
        <h2>Contact Us</h2>
        <div class="title-line"></div>
      </div>
      <p class="section-intro">
        We'd love to hear from you. Reach out to us using the contact information below.
      </p>

      <div class="contact-showcase">
        <div class="contact-grid">
          <article class="contact-card">
            <p class="contact-label">Address</p>
            <p class="contact-value">153 J.P. Rizal St, Mendez, Cavite</p>
          </article>

          <article class="contact-card">
            <p class="contact-label">Phone</p>
            <p class="contact-value"><a href="tel:+63464130429">(046) 413-0429</a></p>
          </article>

          <article class="contact-card">
            <p class="contact-label">Email</p>
            <p class="contact-value">
              <a href="mailto:josonperey@gmail.com">josonperey@gmail.com</a>
            </p>
          </article>
        </div>

        <div class="contact-map" aria-label="Map showing Joson-Perey Dental Clinic location">
          <iframe
            src="https://www.google.com/maps?q=Joson-Perey%20Dental%20Clinic%2C%20Mendez%2C%20Cavite&amp;z=15&amp;output=embed"
            title="Joson-Perey Dental Clinic map"
            loading="lazy"
            allowfullscreen
            referrerpolicy="no-referrer-when-downgrade"
          ></iframe>
        </div>
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

.section-intro {
  max-width: 1020px;
  margin: 0 auto 34px;
  color: #2d4961;
  text-align: center;
  font-size: 1.08rem;
}

.contact {
  background: var(--sky-soft);
}

.contact-showcase {
  padding: 28px;
  border: 1px solid rgba(8, 60, 109, 0.1);
  border-radius: 28px;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.86) 0%, rgba(255, 255, 255, 0.96) 100%);
  box-shadow: 0 22px 50px rgba(8, 60, 109, 0.12);
}

.contact-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 18px;
  margin-bottom: 24px;
}

.contact-card {
  position: relative;
  overflow: hidden;
  padding: 24px 22px;
  border: 1px solid rgba(15, 177, 224, 0.16);
  border-radius: 24px;
  background: linear-gradient(145deg, #ffffff 0%, #f4fbff 100%);
  box-shadow: 0 14px 30px rgba(15, 177, 224, 0.08);
}

.contact-label {
  margin: 0 0 8px;
  color: #4d8aa9;
  font-size: 0.85rem;
  font-weight: 800;
  letter-spacing: 0.16em;
  text-transform: uppercase;
}

.contact-value {
  margin: 0;
  color: var(--ink);
  font-size: 1.1rem;
  font-weight: 700;
  line-height: 1.6;
}

.contact-value a {
  color: inherit;
}

.contact-map {
  overflow: hidden;
  border: 1px solid rgba(8, 60, 109, 0.12);
  border-radius: 24px;
  background: #fff;
  box-shadow: 0 14px 28px rgba(8, 60, 109, 0.1);
}

.contact-map iframe {
  display: block;
  width: 100%;
  height: 340px;
  border: 0;
}

@media (max-width: 960px) {
  .contact-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 640px) {
  .section-shell {
    padding: 70px 0;
  }
}
</style>
