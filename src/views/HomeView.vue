<script setup lang="ts">
import { nextTick, onBeforeUnmount, onMounted, reactive, ref, watch } from 'vue'
import HeaderSection from '@/components/home/HeaderSection.vue'
import HeroSection from '@/components/home/HeroSection.vue'
import ServicesSection from '@/components/home/ServicesSection.vue'
import DoctorsSection from '@/components/home/DoctorsSection.vue'
import AboutSection from '@/components/home/AboutSection.vue'
import ContactSection from '@/components/home/ContactSection.vue'
import FooterSection from '@/components/home/FooterSection.vue'

type ModalName = 'login' | 'forgot' | 'register'

interface LocationItem {
  code: string
  name: string
}

interface RawLocationItem {
  psgcCode?: string
  code?: string
  municipalCityCode?: string
  name?: string
  label?: string
}

const PSGC_BASE_URL = 'https://psgc.cloud/api/v2'
const DEFAULT_CITY_PLACEHOLDER = 'Select province first'
const DEFAULT_BARANGAY_PLACEHOLDER = 'Select city / municipality first'
const LOADING_PROVINCES_MESSAGE = 'Loading Philippine address options…'
const LOAD_PROVINCES_ERROR_MESSAGE = 'Philippine address data could not be loaded right now.'
const LOAD_CITIES_ERROR_MESSAGE = 'Could not load cities and municipalities for the selected province.'
const LOAD_BARANGAYS_ERROR_MESSAGE = 'Could not load barangays for the selected city or municipality.'

const activeModal = ref<ModalName | null>(null)
const previousFocusedElement = ref<HTMLElement | null>(null)
const loginPanelRef = ref<HTMLElement | null>(null)
const forgotPanelRef = ref<HTMLElement | null>(null)
const registerPanelRef = ref<HTMLElement | null>(null)
const showLoginPassword = ref(false)
const showRegisterPassword = ref(false)
const provinceLoading = ref(false)
const addressFeedback = ref('')

const loginForm = reactive({
  email: '',
  password: '',
})

const forgotForm = reactive({
  email: '',
})

const registerForm = reactive({
  firstName: '',
  lastName: '',
  email: '',
  phoneNumber: '',
  dateOfBirth: '',
  gender: '',
  provinceCode: '',
  cityCode: '',
  barangayCode: '',
  preferredContactMethod: '',
  password: '',
})

const provinces = ref<LocationItem[]>([])
const cities = ref<LocationItem[]>([])
const barangays = ref<LocationItem[]>([])

const provinceState = reactive({
  placeholder: 'Loading provinces…',
  disabled: true,
})

const cityState = reactive({
  placeholder: DEFAULT_CITY_PLACEHOLDER,
  disabled: true,
})

const barangayState = reactive({
  placeholder: DEFAULT_BARANGAY_PLACEHOLDER,
  disabled: true,
})

const provinceCache = ref<LocationItem[] | null>(null)
const citiesCache = new Map<string, LocationItem[]>()
const barangaysCache = new Map<string, LocationItem[]>()

const normalizeLocationItems = (payload: unknown): RawLocationItem[] => {
  if (Array.isArray(payload)) {
    return payload as RawLocationItem[]
  }

  if (payload && typeof payload === 'object') {
    const record = payload as { data?: unknown; items?: unknown }

    if (Array.isArray(record.data)) {
      return record.data as RawLocationItem[]
    }

    if (Array.isArray(record.items)) {
      return record.items as RawLocationItem[]
    }

    return [record as RawLocationItem]
  }

  return []
}

const sortLocations = (items: RawLocationItem[]): LocationItem[] =>
  normalizeLocationItems(items)
    .map((item) => ({
      code: item.psgcCode ?? item.code ?? item.municipalCityCode ?? '',
      name: item.name ?? item.label ?? '',
    }))
    .filter((item) => item.code && item.name)
    .sort((first, second) => first.name.localeCompare(second.name))

const getActivePanel = () => {
  if (activeModal.value === 'login') return loginPanelRef.value
  if (activeModal.value === 'forgot') return forgotPanelRef.value
  if (activeModal.value === 'register') return registerPanelRef.value
  return null
}

const focusFirstField = async () => {
  await nextTick()
  const panel = getActivePanel()
  const firstFocusable = panel?.querySelector<HTMLElement>('input, select, button, a')
  firstFocusable?.focus()
}

const fetchJson = async (url: string) => {
  const response = await fetch(url, {
    headers: {
      Accept: 'application/json',
    },
  })

  if (!response.ok) {
    throw new Error(`Request failed with status ${response.status}`)
  }

  return response.json()
}

const resetCityState = (provinceCode = '') => {
  cities.value = []
  registerForm.cityCode = ''
  cityState.placeholder = provinceCode ? 'Loading cities / municipalities…' : DEFAULT_CITY_PLACEHOLDER
  cityState.disabled = true
}

const resetBarangayState = (localityCode = '') => {
  barangays.value = []
  registerForm.barangayCode = ''
  barangayState.placeholder = localityCode ? 'Loading barangays…' : DEFAULT_BARANGAY_PLACEHOLDER
  barangayState.disabled = true
}

const initAddressOptions = async () => {
  if (provinceCache.value) {
    provinces.value = provinceCache.value
    provinceState.placeholder = 'Select province'
    provinceState.disabled = false
    return provinceCache.value
  }

  provinceLoading.value = true
  addressFeedback.value = LOADING_PROVINCES_MESSAGE

  try {
    const payload = await fetchJson(`${PSGC_BASE_URL}/provinces`)
    const items = sortLocations(payload)
    provinceCache.value = items
    provinces.value = items
    provinceState.placeholder = 'Select province'
    provinceState.disabled = false
    addressFeedback.value = ''
    return items
  } catch (error) {
    console.error('Unable to load Philippine address data.', error)
    provinceCache.value = null
    provinces.value = []
    provinceState.placeholder = 'Unable to load provinces'
    provinceState.disabled = true
    resetCityState()
    resetBarangayState()
    addressFeedback.value = LOAD_PROVINCES_ERROR_MESSAGE
    throw error
  } finally {
    provinceLoading.value = false
  }
}

const loadCitiesByProvince = async (provinceCode: string) => {
  if (!provinceCode) {
    return []
  }

  if (citiesCache.has(provinceCode)) {
    return citiesCache.get(provinceCode) ?? []
  }

  const payload = await fetchJson(
    `${PSGC_BASE_URL}/provinces/${encodeURIComponent(provinceCode)}/cities-municipalities`,
  )
  const items = sortLocations(payload)
  citiesCache.set(provinceCode, items)
  return items
}

const loadBarangaysByLocality = async (localityCode: string) => {
  if (!localityCode) {
    return []
  }

  if (barangaysCache.has(localityCode)) {
    return barangaysCache.get(localityCode) ?? []
  }

  const payload = await fetchJson(
    `${PSGC_BASE_URL}/cities-municipalities/${encodeURIComponent(localityCode)}/barangays`,
  )
  const items = sortLocations(payload)
  barangaysCache.set(localityCode, items)
  return items
}

const handleProvinceChange = async () => {
  const provinceCode = registerForm.provinceCode
  resetCityState(provinceCode)
  resetBarangayState()

  if (!provinceCode) {
    addressFeedback.value = ''
    return
  }

  try {
    await initAddressOptions()
    const provinceCities = await loadCitiesByProvince(provinceCode)
    cities.value = provinceCities
    cityState.placeholder = provinceCities.length
      ? 'Select city / municipality'
      : 'No cities / municipalities found'
    cityState.disabled = !provinceCities.length
    addressFeedback.value = ''
  } catch (error) {
    console.error('Unable to load cities / municipalities.', error)
    cities.value = []
    cityState.placeholder = 'Unable to load cities / municipalities'
    cityState.disabled = true
    addressFeedback.value = LOAD_CITIES_ERROR_MESSAGE
  }
}

const handleCityChange = async () => {
  const localityCode = registerForm.cityCode
  resetBarangayState(localityCode)

  if (!localityCode) {
    addressFeedback.value = ''
    return
  }

  try {
    const localityBarangays = await loadBarangaysByLocality(localityCode)
    barangays.value = localityBarangays
    barangayState.placeholder = localityBarangays.length ? 'Select barangay' : 'No barangays found'
    barangayState.disabled = !localityBarangays.length
    addressFeedback.value = ''
  } catch (error) {
    console.error('Unable to load barangays.', error)
    barangays.value = []
    barangayState.placeholder = 'Unable to load barangays'
    barangayState.disabled = true
    addressFeedback.value = LOAD_BARANGAYS_ERROR_MESSAGE
  }
}

const onProvinceFocus = () => {
  if (!provinceCache.value && !provinceLoading.value && !provinceState.disabled) {
    initAddressOptions().catch(() => undefined)
  }
}

const openModal = (modal: ModalName) => {
  if (!activeModal.value) {
    previousFocusedElement.value = document.activeElement as HTMLElement | null
  }

  activeModal.value = modal

  if (modal === 'register') {
    initAddressOptions().catch(() => undefined)
  }
}

const closeModal = () => {
  activeModal.value = null
  previousFocusedElement.value?.focus?.()
}

const handleKeydown = (event: KeyboardEvent) => {
  if (event.key === 'Escape' && activeModal.value) {
    closeModal()
  }
}

watch(activeModal, async (modal) => {
  document.body.classList.toggle('modal-open', Boolean(modal))

  if (modal) {
    await focusFirstField()
  }
})

onMounted(() => {
  document.addEventListener('keydown', handleKeydown)
  initAddressOptions().catch(() => undefined)
})

onBeforeUnmount(() => {
  document.removeEventListener('keydown', handleKeydown)
  document.body.classList.remove('modal-open')
})
</script>

<template>
  <div id="top">
    <div class="top-strip">Opening Hours: 8 AM - 5 PM, closed every Wednesday and Sunday</div>

    <HeaderSection @open-modal="openModal" />

    <main>
      <HeroSection @open-modal="openModal" />
      <ServicesSection />
      <DoctorsSection />
      <AboutSection />
      <ContactSection />
    </main>

    <FooterSection />

    <div
      class="modal-overlay"
      :class="{ 'is-open': Boolean(activeModal) }"
      :aria-hidden="!activeModal"
      @click.self="closeModal"
    >
      <section
        v-if="activeModal === 'login'"
        ref="loginPanelRef"
        class="modal-panel"
        role="dialog"
        tabindex="-1"
        aria-labelledby="loginModalTitle"
      >
        <div class="modal-header">
          <h2 id="loginModalTitle">Login</h2>
          <button class="modal-close" type="button" aria-label="Close login modal" @click="closeModal"></button>
        </div>
        <div class="modal-body">
          <p class="modal-copy">
            Welcome back. Sign in to manage appointments, review your account details, and stay
            connected with your dental care.
          </p>
          <form class="modal-form" @submit.prevent>
            <div class="form-group">
              <label class="form-label" for="loginEmail">Email</label>
              <input
                id="loginEmail"
                v-model="loginForm.email"
                class="field-control"
                name="email"
                type="email"
                placeholder="Enter your email"
                autocomplete="email"
                required
              />
            </div>
            <div class="form-group">
              <label class="form-label" for="loginPassword">Password</label>
              <div class="password-field">
                <input
                  id="loginPassword"
                  v-model="loginForm.password"
                  class="field-control"
                  name="password"
                  :type="showLoginPassword ? 'text' : 'password'"
                  placeholder="Enter your password"
                  autocomplete="current-password"
                  required
                />
                <button
                  class="password-toggle"
                  type="button"
                  aria-controls="loginPassword"
                  :aria-label="showLoginPassword ? 'Hide password' : 'Show password'"
                  :aria-pressed="showLoginPassword"
                  @click="showLoginPassword = !showLoginPassword"
                >
                  {{ showLoginPassword ? 'Hide' : 'Show' }}
                </button>
              </div>
            </div>
            <div class="modal-link-row">
              <a class="modal-link" href="#" @click.prevent="openModal('forgot')">Forgot Password?</a>
            </div>
            <button class="modal-submit" type="submit">Login</button>
          </form>
          <div class="modal-footer-copy">
            Don't have an account?
            <a href="#" @click.prevent="openModal('register')">Register</a>
          </div>
        </div>
      </section>

      <section
        v-if="activeModal === 'forgot'"
        ref="forgotPanelRef"
        class="modal-panel"
        role="dialog"
        tabindex="-1"
        aria-labelledby="forgotModalTitle"
      >
        <div class="modal-header">
          <h2 id="forgotModalTitle">Forgot Password</h2>
          <button class="modal-close" type="button" aria-label="Close login modal" @click="closeModal"></button>
        </div>
        <div class="modal-body">
          <p class="modal-copy">Enter your email to begin your password recovery process.</p>
          <form class="modal-form" @submit.prevent>
            <div class="form-group">
              <label class="form-label" for="forgotEmail">Registered Email</label>
              <input
                id="forgotEmail"
                v-model="forgotForm.email"
                class="field-control"
                name="forgot-email"
                type="email"
                placeholder="Enter your email"
                autocomplete="email"
                required
              />
            </div>
            <button class="modal-submit" type="submit">Send Code</button>
          </form>
          <div class="modal-footer-copy"><a href="#" @click.prevent="openModal('login')">Back to login</a></div>
        </div>
      </section>

      <section
        v-if="activeModal === 'register'"
        ref="registerPanelRef"
        class="modal-panel modal-wide"
        role="dialog"
        tabindex="-1"
        aria-labelledby="registerModalTitle"
      >
        <div class="modal-header">
          <h2 id="registerModalTitle">Register</h2>
          <button class="modal-close" type="button" aria-label="Close login modal" @click="closeModal"></button>
        </div>
        <div class="modal-body">
          <p class="modal-copy">
            Create your patient account to make future bookings easier and keep your contact
            details ready for the clinic.
          </p>
          <form class="modal-form" @submit.prevent>
            <div class="form-grid">
              <div class="form-group">
                <label class="form-label" for="registerFirstName">First Name</label>
                <input
                  id="registerFirstName"
                  v-model="registerForm.firstName"
                  class="field-control"
                  name="first-name"
                  type="text"
                  placeholder="Enter your first name"
                  autocomplete="given-name"
                  required
                />
              </div>
              <div class="form-group">
                <label class="form-label" for="registerLastName">Last Name</label>
                <input
                  id="registerLastName"
                  v-model="registerForm.lastName"
                  class="field-control"
                  name="last-name"
                  type="text"
                  placeholder="Enter your last name"
                  autocomplete="family-name"
                  required
                />
              </div>
              <div class="form-group">
                <label class="form-label" for="registerEmail">Email</label>
                <input
                  id="registerEmail"
                  v-model="registerForm.email"
                  class="field-control"
                  name="email"
                  type="email"
                  placeholder="Enter your email"
                  autocomplete="email"
                  required
                />
              </div>
              <div class="form-group">
                <label class="form-label" for="registerPhone">Phone Number</label>
                <input
                  id="registerPhone"
                  v-model="registerForm.phoneNumber"
                  class="field-control"
                  name="phone-number"
                  type="tel"
                  placeholder="Enter your phone number"
                  autocomplete="tel"
                  required
                />
              </div>
              <div class="form-group">
                <label class="form-label" for="registerDob">Date of Birth</label>
                <input
                  id="registerDob"
                  v-model="registerForm.dateOfBirth"
                  class="field-control"
                  name="date-of-birth"
                  type="date"
                  required
                />
              </div>
              <div class="form-group">
                <label class="form-label" for="registerGender">Gender</label>
                <select id="registerGender" v-model="registerForm.gender" class="field-control" required>
                  <option value="">Select gender</option>
                  <option value="female">Female</option>
                  <option value="male">Male</option>
                  <option value="prefer-not-to-say">Prefer not to say</option>
                </select>
              </div>
              <div class="form-group">
                <label class="form-label" for="registerProvince">Province</label>
                <select
                  id="registerProvince"
                  v-model="registerForm.provinceCode"
                  class="field-control"
                  :disabled="provinceLoading || provinceState.disabled"
                  required
                  @focus="onProvinceFocus"
                  @change="handleProvinceChange"
                >
                  <option value="">{{ provinceState.placeholder }}</option>
                  <option v-for="province in provinces" :key="province.code" :value="province.code">
                    {{ province.name }}
                  </option>
                </select>
              </div>
              <div class="form-group">
                <label class="form-label" for="registerCity">City / Municipality</label>
                <select
                  id="registerCity"
                  v-model="registerForm.cityCode"
                  class="field-control"
                  :disabled="cityState.disabled"
                  required
                  @change="handleCityChange"
                >
                  <option value="">{{ cityState.placeholder }}</option>
                  <option v-for="city in cities" :key="city.code" :value="city.code">
                    {{ city.name }}
                  </option>
                </select>
              </div>
              <div class="form-group">
                <label class="form-label" for="registerBarangay">Barangay</label>
                <select
                  id="registerBarangay"
                  v-model="registerForm.barangayCode"
                  class="field-control"
                  :disabled="barangayState.disabled"
                  required
                >
                  <option value="">{{ barangayState.placeholder }}</option>
                  <option v-for="barangay in barangays" :key="barangay.code" :value="barangay.code">
                    {{ barangay.name }}
                  </option>
                </select>
              </div>
              <div class="form-group">
                <label class="form-label" for="registerPreferredContact">Preferred Contact Method</label>
                <select
                  id="registerPreferredContact"
                  v-model="registerForm.preferredContactMethod"
                  class="field-control"
                  required
                >
                  <option value="">Choose one for appointment reminders</option>
                  <option value="sms">SMS</option>
                  <option value="email">Email</option>
                  <option value="do-not-contact">Do Not Contact Me</option>
                </select>
              </div>
              <div class="form-group full">
                <label class="form-label" for="registerPassword">Password</label>
                <div class="password-field">
                  <input
                    id="registerPassword"
                    v-model="registerForm.password"
                    class="field-control"
                    name="register-password"
                    :type="showRegisterPassword ? 'text' : 'password'"
                    placeholder="Create a password"
                    autocomplete="new-password"
                    required
                  />
                  <button
                    class="password-toggle"
                    type="button"
                    aria-controls="registerPassword"
                    :aria-label="showRegisterPassword ? 'Hide password' : 'Show password'"
                    :aria-pressed="showRegisterPassword"
                    @click="showRegisterPassword = !showRegisterPassword"
                  >
                    {{ showRegisterPassword ? 'Hide' : 'Show' }}
                  </button>
                </div>
              </div>
            </div>
            <p class="address-feedback">{{ addressFeedback }}</p>
            <button class="modal-submit" type="submit">Create Account</button>
          </form>
          <div class="modal-footer-copy">
            Already have an account? <a href="#" @click.prevent="openModal('login')">Login</a>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
.modal-panel {
  display: flex;
  flex-direction: column;
}

.modal-wide {
  width: min(100%, 940px);
}

.modal-header h2 {
  margin: 0;
  font-size: clamp(1.5rem, 3vw, 2rem);
  font-weight: 800;
  line-height: 1.15;
  letter-spacing: -0.03em;
}

.modal-copy {
  max-width: 620px;
  margin: 0 auto 22px;
  color: #557085;
  text-align: center;
  font-size: 0.98rem;
}

.modal-form {
  display: grid;
  gap: 16px;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 16px;
}

.modal-link-row {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 22px;
  flex-wrap: wrap;
  margin: -2px 0 6px;
}

.modal-link {
  color: #0d8cc3;
  font-size: 0.95rem;
  font-weight: 700;
  transition: color 0.2s ease;
}

.modal-link:hover,
.modal-footer-copy a:hover {
  color: var(--navy);
}

.modal-footer-copy {
  margin-top: 18px;
  color: #516777;
  text-align: center;
  font-size: 1rem;
}

.modal-footer-copy a {
  color: #0d8cc3;
  font-weight: 800;
}

.address-feedback {
  min-height: 1.3em;
  margin: 2px 2px 0;
  color: #6e8596;
  font-size: 0.9rem;
}

@media (max-width: 760px) {
  .form-grid {
    grid-template-columns: 1fr;
  }

  .modal-link-row {
    gap: 12px 18px;
  }
}
</style>
