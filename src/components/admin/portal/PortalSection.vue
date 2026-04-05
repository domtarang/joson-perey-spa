<script setup lang="ts">
import { nextTick, onBeforeUnmount, onMounted, reactive, ref, watch } from 'vue'

const showPassword = ref(false)
const isForgotModalOpen = ref(false)
const forgotPanelRef = ref<HTMLElement | null>(null)
const resetEmail = ref('')
const loginStatus = ref('')
const resetStatus = ref('')

const loginForm = reactive({
  email: '',
  password: '',
  rememberMe: false,
})

const handleLoginSubmit = () => {
  loginStatus.value = 'Frontend ready. Connect this form to your admin authentication endpoint.'
}

const openForgotModal = async () => {
  isForgotModalOpen.value = true
  resetStatus.value = ''
  await nextTick()
  forgotPanelRef.value?.querySelector<HTMLElement>('#resetEmail')?.focus()
}

const closeForgotModal = () => {
  isForgotModalOpen.value = false
  resetStatus.value = ''
}

const handleResetSubmit = () => {
  resetStatus.value = ''

  if (!resetEmail.value.trim()) {
    resetStatus.value = 'Please enter the registered admin email.'
    return
  }

  resetStatus.value = `Reset code request captured for ${resetEmail.value.trim()}.`
}

const handleKeydown = (event: KeyboardEvent) => {
  if (event.key === 'Escape' && isForgotModalOpen.value) {
    closeForgotModal()
  }
}

watch(isForgotModalOpen, (isOpen) => {
  document.body.classList.toggle('modal-open', isOpen)
})

onMounted(() => {
  document.addEventListener('keydown', handleKeydown)
})

onBeforeUnmount(() => {
  document.removeEventListener('keydown', handleKeydown)
  document.body.classList.remove('modal-open')
})
</script>

<template>
  <main class="admin-shell">
    <div class="container admin-layout">
      <section class="login-card" aria-labelledby="adminLoginTitle">
        <div class="card-badge">Secure Admin Login</div>
        <h2 id="adminLoginTitle">Welcome back</h2>
        <p>Enter your admin credentials to continue to the dashboard.</p>

        <form @submit.prevent="handleLoginSubmit">
          <div class="form-group login-form-group">
            <label class="form-label" for="adminEmail">Email</label>
            <input
              id="adminEmail"
              v-model="loginForm.email"
              class="field-control"
              name="email"
              type="email"
              placeholder="Enter admin email"
              autocomplete="email"
              required
            />
          </div>

          <div class="form-group login-form-group">
            <label class="form-label" for="adminPassword">Password</label>
            <div class="password-field">
              <input
                id="adminPassword"
                v-model="loginForm.password"
                class="field-control"
                name="password"
                :type="showPassword ? 'text' : 'password'"
                placeholder="Enter password"
                autocomplete="current-password"
                required
              />
              <button
                class="password-toggle"
                type="button"
                aria-controls="adminPassword"
                :aria-label="showPassword ? 'Hide password' : 'Show password'"
                :aria-pressed="showPassword"
                @click="showPassword = !showPassword"
              >
                {{ showPassword ? 'Hide' : 'Show' }}
              </button>
            </div>
          </div>

          <div class="row-between">
            <label class="remember-wrap" for="rememberMe">
              <input id="rememberMe" v-model="loginForm.rememberMe" name="rememberMe" type="checkbox" />
              <span>Keep me signed in</span>
            </label>
            <a class="forgot-link" href="#" @click.prevent="openForgotModal">Forgot Password?</a>
          </div>

          <button class="login-btn" type="submit">Login</button>
          <div class="status-message" :class="{ 'is-visible': Boolean(loginStatus) }" role="status" aria-live="polite">
            {{ loginStatus }}
          </div>
        </form>
      </section>

      <section class="welcome-panel" aria-label="Admin portal overview">
        <h1>Secure access for clinic personnel</h1>
        <p>
          Sign in to manage clinic operations, monitor appointment-related activity, and keep the
          dental practice organized through a clean and focused admin portal.
        </p>

        <div class="highlight-grid">
          <article class="highlight-card">
            <div class="highlight-icon" aria-hidden="true">📅</div>
            <h2>Appointment Oversight</h2>
            <p>Review schedules, manage incoming requests, and keep daily bookings well organized.</p>
          </article>

          <article class="highlight-card">
            <div class="highlight-icon" aria-hidden="true">🛡️</div>
            <h2>Protected Access</h2>
            <p>
              Dedicated login page for admins only, separated from the clinic’s public-facing
              website.
            </p>
          </article>

          <article class="highlight-card">
            <div class="highlight-icon" aria-hidden="true">🗂️</div>
            <h2>Patient Records</h2>
            <p>Keep essential patient details organized and easy to review within the clinic portal.</p>
          </article>
        </div>
      </section>
    </div>

    <div
      class="modal-overlay"
      :class="{ 'is-open': isForgotModalOpen }"
      :aria-hidden="!isForgotModalOpen"
      @click.self="closeForgotModal"
    >
      <section
        v-if="isForgotModalOpen"
        ref="forgotPanelRef"
        class="modal-panel admin-modal-panel"
        role="dialog"
        tabindex="-1"
        aria-modal="true"
        aria-labelledby="forgotPasswordTitle"
      >
        <div class="modal-header">
          <h3 id="forgotPasswordTitle">Forgot Password</h3>
          <button
            class="modal-close admin-modal-close"
            type="button"
            aria-label="Close forgot password modal"
            @click="closeForgotModal"
          ></button>
        </div>
        <div class="modal-body admin-modal-body">
          <p>Enter the registered admin email to receive a reset code.</p>

          <form @submit.prevent="handleResetSubmit">
            <div class="form-group login-form-group">
              <label class="form-label" for="resetEmail">Registered Admin Email</label>
              <input
                id="resetEmail"
                v-model="resetEmail"
                class="field-control"
                name="resetEmail"
                type="email"
                placeholder="Enter registered admin email"
                autocomplete="email"
                required
              />
            </div>

            <button class="modal-submit" type="submit">Send Reset Code</button>
            <div class="status-message" :class="{ 'is-visible': Boolean(resetStatus) }" role="status" aria-live="polite">
              {{ resetStatus }}
            </div>
          </form>
        </div>
      </section>
    </div>
  </main>
</template>

<style scoped>
.admin-shell {
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  flex: 1;
  padding: 72px 0 88px;
}

.admin-shell::before,
.admin-shell::after {
  content: '';
  position: absolute;
  border-radius: 50%;
  pointer-events: none;
  opacity: 0.6;
  filter: blur(4px);
}

.admin-shell::before {
  top: 70px;
  left: -90px;
  width: 240px;
  height: 240px;
  background: radial-gradient(circle, rgba(15, 177, 224, 0.26), transparent 68%);
}

.admin-shell::after {
  right: -80px;
  bottom: 20px;
  width: 280px;
  height: 280px;
  background: radial-gradient(circle, rgba(8, 60, 109, 0.18), transparent 70%);
}

.admin-layout {
  position: relative;
  z-index: 1;
  display: grid;
  grid-template-columns: minmax(360px, 470px) minmax(0, 1fr);
  gap: 34px;
  align-items: center;
}

.welcome-panel {
  padding: 22px 10px 22px 0;
}

.welcome-panel h1 {
  margin: 0 0 16px;
  color: var(--navy);
  font-size: clamp(2.3rem, 5vw, 4rem);
  font-weight: 800;
  line-height: 1.08;
  letter-spacing: -0.04em;
  text-transform: uppercase;
}

.welcome-panel p {
  max-width: 660px;
  margin: 0 0 26px;
  color: #335167;
  font-size: 1.08rem;
}

.highlight-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 16px;
  max-width: 760px;
}

.highlight-card {
  padding: 18px 18px 20px;
  border: 1px solid rgba(8, 60, 109, 0.1);
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.84);
  box-shadow: 0 12px 24px rgba(8, 60, 109, 0.06);
  backdrop-filter: blur(6px);
}

.highlight-icon {
  display: grid;
  place-items: center;
  width: 48px;
  height: 48px;
  margin-bottom: 14px;
  border-radius: 16px;
  background: linear-gradient(135deg, rgba(15, 177, 224, 0.16), rgba(8, 60, 109, 0.12));
  color: var(--navy);
  font-size: 1.35rem;
  font-weight: 800;
}

.highlight-card h2 {
  margin: 0 0 8px;
  color: var(--navy);
  font-size: 1.05rem;
  line-height: 1.2;
}

.highlight-card p {
  margin: 0;
  color: #4f6473;
  font-size: 0.95rem;
}

.login-card {
  position: relative;
  overflow: hidden;
  padding: 28px;
  border: 1px solid rgba(8, 60, 109, 0.12);
  border-radius: 28px;
  background: rgba(255, 255, 255, 0.94);
  box-shadow: var(--shadow);
  backdrop-filter: blur(10px);
}

.login-card::before {
  content: '';
  position: absolute;
  inset: 0 0 auto;
  height: 8px;
  background: linear-gradient(90deg, var(--navy) 0%, var(--sky) 100%);
}

.card-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 16px;
  padding: 8px 12px;
  border-radius: 999px;
  background: rgba(15, 177, 224, 0.1);
  color: #0a6796;
  font-size: 0.83rem;
  font-weight: 800;
  letter-spacing: 0.04em;
  text-transform: uppercase;
}

.card-badge::before {
  content: '';
  width: 9px;
  height: 9px;
  border-radius: 50%;
  background: var(--sky);
}

.login-card h2 {
  margin: 0 0 10px;
  color: var(--navy);
  font-size: 2rem;
  line-height: 1.1;
  letter-spacing: -0.03em;
}

.login-card > p {
  margin: 0 0 24px;
  color: #526979;
  font-size: 1rem;
}

.login-form-group {
  margin-bottom: 16px;
}

.row-between {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 14px;
  margin: 4px 0 20px;
  flex-wrap: wrap;
}

.remember-wrap {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  color: #4b6272;
  font-size: 0.94rem;
  font-weight: 600;
}

.remember-wrap input {
  width: 17px;
  height: 17px;
  accent-color: var(--sky);
}

.forgot-link {
  color: #0b86bf;
  font-size: 0.94rem;
  font-weight: 700;
}

.forgot-link:hover {
  color: var(--navy);
}

.login-btn {
  width: 100%;
  min-height: 56px;
  border: 0;
  border-radius: 16px;
  background: linear-gradient(135deg, #0fb1e0 0%, #0a8fd0 100%);
  color: #fff;
  font-size: 1rem;
  font-weight: 800;
  letter-spacing: 0.01em;
  box-shadow: 0 16px 28px rgba(15, 177, 224, 0.2);
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.login-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 18px 32px rgba(15, 177, 224, 0.24);
}

.admin-modal-panel {
  width: min(100%, 480px);
}

.admin-modal-close {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0;
  font-size: 0;
  line-height: 0;
}

.admin-modal-close::before {
  content: '';
  width: 18px;
  height: 18px;
  border-radius: 2px;
  background:
    linear-gradient(currentColor 0 0) center / 100% 2.5px no-repeat,
    linear-gradient(currentColor 0 0) center / 2.5px 100% no-repeat;
  transform: rotate(45deg);
  transform-origin: center;
}

.admin-modal-close:focus-visible {
  outline: none;
  box-shadow:
    inset 0 0 0 1px rgba(255, 255, 255, 0.22),
    0 0 0 4px rgba(255, 255, 255, 0.2);
}

.admin-modal-body p {
  margin: 0 0 18px;
  color: #557085;
  font-size: 0.98rem;
}

.modal-header h3 {
  margin: 0;
  font-size: 1.7rem;
  font-weight: 800;
  line-height: 1.15;
  letter-spacing: -0.03em;
}

@media (max-width: 1040px) {
  .admin-layout {
    grid-template-columns: 1fr;
  }

  .welcome-panel {
    padding-right: 0;
  }

  .highlight-grid {
    max-width: none;
  }
}

@media (max-width: 820px) {
  .highlight-grid {
    grid-template-columns: 1fr;
  }

  .admin-shell {
    padding-top: 56px;
  }
}

@media (max-width: 640px) {
  .welcome-panel h1 {
    font-size: 2.2rem;
  }

  .login-card {
    padding: 22px 18px;
  }

  .row-between {
    align-items: flex-start;
  }

  .admin-modal-close::before {
    width: 16px;
    height: 16px;
  }
}
</style>
