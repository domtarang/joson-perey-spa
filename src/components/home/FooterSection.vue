<script setup lang="ts"></script>

<template>
  <footer class="home-footer">
    <div class="container">
      <div class="footer-grid">
        <div>
          <div class="footer-brand">Joson-Perey Dental Clinic</div>
          <div class="footer-list">
            <div>
              Caring for brighter smiles in Mendez, Cavite with trusted, patient-focused dental
              care.
            </div>
            <div>Address: 153 J.P. Rizal St, Mendez Cavite</div>
            <div>Phone: (046) 413-0429</div>
            <div>Email: josonperey@gmail.com</div>
          </div>
        </div>

        <div>
          <div class="footer-title">Quick Links</div>
          <div class="footer-list">
            <a href="#home">Home</a>
            <a href="#services">Services</a>
            <a href="#doctors">Doctors</a>
            <a href="#about">About Us</a>
            <a href="#contact">Contact</a>
          </div>
        </div>

        <div>
          <div class="footer-title">Clinic Hours</div>
          <div class="footer-list">
            <div>Monday - Tuesday: 8:00 AM - 5:00 PM</div>
            <div>Wednesday: Closed</div>
            <div>Thursday - Saturday: 8:00 AM - 5:00 PM</div>
            <div>Sunday: Closed</div>
          </div>
        </div>
      </div>

      <div class="footer-bottom">© 2026 Joson-Perey Dental Clinic. All rights reserved.</div>
    </div>
  </footer>
</template>

<style scoped>
.home-footer {
  padding: 30px 0 18px;
  background: var(--navy);
  color: rgba(255, 255, 255, 0.92);
}

.footer-grid {
  display: grid;
  grid-template-columns: 1.3fr 0.8fr 1fr;
  gap: 28px;
}

.footer-brand {
  margin-bottom: 10px;
  color: #fff;
  font-size: 1.6rem;
  font-weight: 800;
}

.footer-title {
  margin: 0 0 12px;
  color: #fff;
  font-size: 1.1rem;
  font-weight: 800;
}

.footer-list {
  display: grid;
  gap: 8px;
  color: rgba(255, 255, 255, 0.88);
  font-size: 0.98rem;
}

.footer-bottom {
  margin-top: 24px;
  padding-top: 16px;
  border-top: 1px solid rgba(255, 255, 255, 0.16);
  color: rgba(255, 255, 255, 0.8);
  text-align: center;
  font-size: 0.96rem;
}

@media (max-width: 1080px) {
  .footer-grid {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 820px) {
  .footer-grid {
    grid-template-columns: 1fr;
  }
}
</style>
