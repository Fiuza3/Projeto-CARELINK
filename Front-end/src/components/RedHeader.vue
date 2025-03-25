<script setup>
import { ref } from 'vue'

const isMenuOpen = ref(false)

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}
</script>

<template>
  <header class="red-header">
    <div class="header-content">
      <h1 class="title">Carelink</h1>
      <button class="menu-button" @click="toggleMenu">
        <div class="hamburger" :class="{ 'open': isMenuOpen }">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </button>
    </div>
    <div class="menu-overlay" :class="{ 'open': isMenuOpen }">
      <div class="menu-content">
        <!-- Add menu items here -->
        <nav>
          <ul>
            <li><a href="#">Home</a></li>
            <li><a href="#">About</a></li>
            <li><a href="#">Services</a></li>
            <li><a href="#">Contact</a></li>
          </ul>
        </nav>
      </div>
    </div>
  </header>
</template>

<style scoped>
.red-header {
  width: 100%;
  height: calc(100px * 100vh / 1080);
  min-height: 40px;
  background-color: red;
  position: relative;
}

.header-content {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  padding: 0 20px;
}

.title {
  color: white;
  font-size: 2em;
  margin: 0;
}

.menu-button {
  position: absolute;
  right: 20px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 10px;
  z-index: 1000;
}

.hamburger {
  width: 30px;
  height: 20px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.hamburger span {
  display: block;
  width: 100%;
  height: 3px;
  background-color: white;
  transition: all 0.3s ease-in-out;
}

.hamburger.open span:nth-child(1) {
  transform: translateY(8.5px) rotate(45deg);
}

.hamburger.open span:nth-child(2) {
  opacity: 0;
}

.hamburger.open span:nth-child(3) {
  transform: translateY(-8.5px) rotate(-45deg);
}

.menu-overlay {
  position: fixed;
  top: 0;
  right: -50%;
  width: 50%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.9);
  transition: right 0.3s ease-in-out;
  z-index: 999;
}

.menu-overlay.open {
  right: 0;
}

.menu-content {
  padding: 100px 40px;
  color: white;
}

.menu-content nav ul {
  list-style: none;
  padding: 0;
}

.menu-content nav ul li {
  margin: 20px 0;
}

.menu-content nav ul li a {
  color: white;
  text-decoration: none;
  font-size: 1.5em;
  transition: color 0.3s ease;
}

.menu-content nav ul li a:hover {
  color: #ff6b6b;
}

@media (max-width: 768px) {
  .menu-overlay {
    width: 100%;
    right: -100%;
  }
  
  .title {
    font-size: 1.5em;
  }
}
</style>