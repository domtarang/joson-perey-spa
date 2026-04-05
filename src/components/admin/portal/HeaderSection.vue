<script setup lang="ts">
import logo from '@/assets/logo.png'
</script>

<template>
  <header class="site-header">
    <div class="container nav">
      <a class="brand" href="/" aria-label="Joson-Perey Dental Clinic">
        <img :src="logo" alt="Joson-Perey Dental Clinic logo" />
        <span>Joson-Perey</span>
      </a>

      <div class="nav-actions">
        <a class="nav-cta" href="/" target="_blank" rel="noopener noreferrer">Open Public Website</a>
      </div>
    </div>
  </header>
</template>

<style scoped>
.nav-actions {
  display: flex;
  align-items: center;
  gap: 14px;
  flex-wrap: wrap;
}

.nav-cta {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 48px;
  padding: 12px 20px;
  border: 0;
  border-radius: 10px;
  background: #fff;
  color: var(--navy);
  font-size: 0.94rem;
  font-weight: 800;
  line-height: 1;
  text-transform: uppercase;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.12);
}

@media (max-width: 820px) {
  .nav-actions {
    justify-content: center;
  }
}

@media (max-width: 640px) {
  .nav-cta {
    width: 100%;
  }
}
</style>
