<script setup lang="ts">
import logo from '@/assets/logo.png'

defineEmits<{
  (event: 'open-modal', modal: 'login' | 'register' | 'forgot'): void
}>()
</script>

<template>
  <header class="site-header">
    <div class="container nav">
      <a class="brand" href="#top" aria-label="Joson-Perey Dental Clinic">
        <img :src="logo" alt="Joson-Perey Dental Clinic logo" />
        <span>Joson-Perey</span>
      </a>

      <nav class="nav-links" aria-label="Primary navigation">
        <a href="#home">Home</a>
        <a href="#services">Services</a>
        <a href="#doctors">Doctors</a>
        <a href="#about">About Us</a>
        <a href="#contact">Contact</a>
        <button class="book-btn" type="button" @click="$emit('open-modal', 'login')">
          Book Appointment
        </button>
      </nav>
    </div>
  </header>
</template>

<style scoped>
.nav-links {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 26px;
  flex-wrap: wrap;
}

.nav-links a:not(.book-btn) {
  color: #fff;
  font-size: 0.98rem;
  font-weight: 700;
}

.book-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 170px;
  min-height: 52px;
  padding: 14px 26px;
  border: 0;
  border-radius: 10px;
  background: #fff;
  color: var(--navy);
  font-size: 0.98rem;
  font-weight: 800;
  line-height: 1;
  white-space: nowrap;
  text-transform: uppercase;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.12);
  cursor: pointer;
}

@media (max-width: 820px) {
  .nav-links {
    justify-content: center;
  }
}

@media (max-width: 640px) {
  .book-btn {
    width: 100%;
  }
}
</style>
