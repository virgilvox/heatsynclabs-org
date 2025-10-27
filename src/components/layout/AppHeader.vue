<template>
  <header class="header">
    <BaseContainer>
      <nav class="nav">
        <router-link to="/" class="nav__logo">
          <HeatSyncLogo />
        </router-link>

        <ul class="nav__links">
          <li><router-link to="/about" class="nav__link">About</router-link></li>
          <li><router-link to="/membership" class="nav__link">Membership</router-link></li>
          <li><router-link to="/calendar" class="nav__link">Events</router-link></li>
          <li><a href="https://www.hackster.io/heatsync-labs1/" class="nav__link" target="_blank" rel="noopener">Projects</a></li>
          <li><a href="https://wiki.heatsynclabs.org" class="nav__link" target="_blank" rel="noopener">Wiki</a></li>
          <li><router-link to="/classes" class="nav__link">Classes</router-link></li>
          <li><router-link to="/support" class="nav__link">Support Us</router-link></li>
        </ul>

        <button
          class="nav__mobile-toggle"
          @click="toggleMobileMenu"
          :aria-expanded="isMobileMenuOpen"
          aria-label="Toggle navigation menu"
        >
          <span class="nav__mobile-toggle-line"></span>
          <span class="nav__mobile-toggle-line"></span>
          <span class="nav__mobile-toggle-line"></span>
        </button>
      </nav>

      <!-- Mobile Menu -->
      <div v-if="isMobileMenuOpen" class="nav__mobile-menu">
        <ul class="nav__mobile-links">
          <li><router-link to="/about" class="nav__mobile-link" @click="closeMobileMenu">About</router-link></li>
          <li><router-link to="/membership" class="nav__mobile-link" @click="closeMobileMenu">Membership</router-link></li>
          <li><router-link to="/calendar" class="nav__mobile-link" @click="closeMobileMenu">Events</router-link></li>
          <li><a href="https://www.hackster.io/heatsync-labs1/" class="nav__mobile-link" target="_blank" rel="noopener">Projects</a></li>
          <li><a href="https://wiki.heatsynclabs.org" class="nav__mobile-link" target="_blank" rel="noopener">Wiki</a></li>
          <li><router-link to="/classes" class="nav__mobile-link" @click="closeMobileMenu">Classes</router-link></li>
          <li><router-link to="/support" class="nav__mobile-link" @click="closeMobileMenu">Support Us</router-link></li>
        </ul>
      </div>
    </BaseContainer>
  </header>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import BaseContainer from '../base/BaseContainer.vue'
import HeatSyncLogo from '../HeatSyncLogo.vue'

const isMobileMenuOpen = ref(false)

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}
</script>

<style scoped>
.header {
  background: var(--cream);
  border-bottom: 1px solid var(--warm-gray);
  box-shadow: 0 1px 4px var(--shadow-light);
  position: sticky;
  top: 0;
  z-index: var(--z-overlay);
}

.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: var(--header-height);
}

.nav__logo {
  text-decoration: none;
  color: inherit;
}

.nav__links {
  display: flex;
  gap: var(--space-8);
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav__link {
  color: var(--graphite);
  text-decoration: none;
  font-size: var(--text-sm);
  font-family: var(--font-mono);
  letter-spacing: var(--tracking-wide);
  transition: color var(--transition-base);
  position: relative;
  padding: var(--space-2) 0;
}

.nav__link::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 1px;
  background: var(--accent-rust);
  transition: width var(--transition-base);
}

.nav__link:hover,
.nav__link.router-link-active {
  color: var(--accent-rust);
}

.nav__link:hover::after,
.nav__link.router-link-active::after {
  width: 100%;
}

/* Mobile toggle button */
.nav__mobile-toggle {
  display: none;
  flex-direction: column;
  gap: 4px;
  background: none;
  border: none;
  padding: var(--space-2);
  cursor: pointer;
}

.nav__mobile-toggle-line {
  width: 20px;
  height: 2px;
  background: var(--graphite);
  transition: all var(--transition-base);
}

/* Mobile menu */
.nav__mobile-menu {
  display: none;
  background: var(--cream);
  border-top: 1px solid var(--warm-gray);
  padding: var(--space-4) 0;
}

.nav__mobile-links {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav__mobile-link {
  color: var(--graphite);
  text-decoration: none;
  font-size: var(--text-base);
  font-family: var(--font-mono);
  letter-spacing: var(--tracking-wide);
  padding: var(--space-3) 0;
  border-bottom: 1px solid transparent;
  transition: all var(--transition-base);
}

.nav__mobile-link:hover,
.nav__mobile-link.router-link-active {
  color: var(--accent-rust);
  border-bottom-color: var(--accent-rust);
}

/* Responsive */
@media (max-width: 768px) {
  .nav__links {
    display: none;
  }

  .nav__mobile-toggle {
    display: flex;
  }

  .nav__mobile-menu {
    display: block;
  }
}

/* Mobile toggle animation */
@media (max-width: 768px) {
  .nav__mobile-toggle[aria-expanded="true"] .nav__mobile-toggle-line:nth-child(1) {
    transform: rotate(45deg) translate(5px, 5px);
  }

  .nav__mobile-toggle[aria-expanded="true"] .nav__mobile-toggle-line:nth-child(2) {
    opacity: 0;
  }

  .nav__mobile-toggle[aria-expanded="true"] .nav__mobile-toggle-line:nth-child(3) {
    transform: rotate(-45deg) translate(7px, -6px);
  }
}
</style>