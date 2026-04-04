<template>
  <div class="admin-page">
    <div class="top-strip admin-top-strip">Admin Portal | Authorized Personnel Only</div>
    <HeaderSection />
    <PortalSection />
    <FooterSection />
  </div>
</template>

<script setup lang="ts">
import HeaderSection from '@/components/admin/portal/HeaderSection.vue'
import PortalSection from '@/components/admin/portal/PortalSection.vue'
import FooterSection from '@/components/admin/portal/FooterSection.vue'
</script>

<style scoped>
.admin-page {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background:
    radial-gradient(circle at top left, rgba(15, 177, 224, 0.12), transparent 24%),
    radial-gradient(circle at bottom right, rgba(8, 60, 109, 0.08), transparent 28%),
    linear-gradient(180deg, #f7fbff 0%, #edf6fb 100%);
}

.admin-top-strip {
  font-weight: 700;
}
</style>
