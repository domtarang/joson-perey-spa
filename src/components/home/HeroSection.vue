<template>
  <section id="home" class="hero">
    <div class="hero-inner">
      <h1>Quality Dentistry to All Ages</h1>
      <p>
        Your oral health is our passion, and we believe that a radiant smile can boost your
        confidence and overall well-being.
      </p>
      <div class="hero-actions">
        <button class="btn btn-light" type="button" @click="$emit('open-modal', 'login')">
          Login
        </button>
        <button class="btn" type="button" @click="$emit('open-modal', 'register')">
          Sign Up
        </button>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
defineEmits<{
  (event: 'open-modal', modal: 'login' | 'register' | 'forgot'): void
}>()
</script>

<style scoped>
.hero {
  display: grid;
  place-items: center;
  min-height: 560px;
  padding: 80px 16px;
  background:
    radial-gradient(circle at top left, rgba(255, 255, 255, 0.16), transparent 28%),
    radial-gradient(circle at bottom right, rgba(255, 255, 255, 0.1), transparent 24%),
    linear-gradient(135deg, #0a335c 0%, #0b5d91 46%, #129dca 100%);
  color: #fff;
  text-align: center;
}

.hero-inner {
  width: min(980px, 100%);
}

.hero h1 {
  margin: 0 0 14px;
  font-size: clamp(2rem, 5vw, 4rem);
  font-weight: 800;
  line-height: 1.1;
  letter-spacing: -0.03em;
  text-transform: uppercase;
}

.hero p {
  max-width: 900px;
  margin: 0 auto;
  color: rgba(255, 255, 255, 0.95);
  font-size: clamp(1rem, 2vw, 1.42rem);
}

.hero-actions {
  display: flex;
  justify-content: center;
  gap: 14px;
  flex-wrap: wrap;
  margin-top: 28px;
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 130px;
  padding: 14px 26px;
  border: 2px solid rgba(255, 255, 255, 0.7);
  border-radius: 10px;
  background: transparent;
  color: #fff;
  font-size: 0.98rem;
  font-weight: 800;
  text-transform: uppercase;
  cursor: pointer;
}

.btn-light {
  border-color: #fff;
  background: #fff;
  color: var(--navy);
}

@media (max-width: 640px) {
  .hero {
    min-height: 480px;
  }

  .hero h1 {
    font-size: 2rem;
  }
}
</style>
