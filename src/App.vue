<script setup>

import { ref, computed } from 'vue'
import Home from './components/HomePage.vue'
import AmazingCalculator from './components/AmazingCalculator.vue'
import PasswordGenerator from './components/PasswordGenerator.vue'
import AccordionComponent from './components/AccordionComponent.vue'
import FormValidation from './components/FormValidation.vue'
import GithubUsersSearch from './components/GithubUsersSearch.vue'
import ProgressSteps from './components/ProgressSteps.vue'
import TodoList from './components/TodoList.vue'
import DadJokes from './components/DadJokes.vue'
import RandomQuoteGenerator from './components/RandomQuoteGenerator.vue'
import WikiComponent from './components/WikiComponent.vue'

const routes = {
  '/': Home,
  '/AmazingCalculator': AmazingCalculator,
  '/AccordionComponent': AccordionComponent,
  '/FormValidation': FormValidation,
  '/GithubUsersSearch': GithubUsersSearch,
  '/PasswordGenerator': PasswordGenerator,
  '/TodoList': TodoList,
  '/DadJokes': DadJokes,
  '/RandomQuoteGenerator': RandomQuoteGenerator,
  '/WikiComponent': WikiComponent,
  '/ProgressSteps': ProgressSteps
}

const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/']  
})
 
 

  window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const isActive = (path) => {
  return currentPath.value === '#' + path
}
const isMenuOpen = ref(false)

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
  
}

</script>

<template>
  <nav class="navbar">
    <div class="menu-icon" @click="toggleMenu">
      <div class="bar"></div>
      <div class="bar"></div>
      <div class="bar"></div>
    </div>
    <ul class="navbar-nav" :class="{ 'active': isMenuOpen }">
      <li :class="{ active: isActive('/') }"><a href="#/" class="nav-item home">Home</a></li>
      <li :class="{ active: isActive('/AmazingCalculator') }"><a href="#/AmazingCalculator" class="nav-item calculator">AmazingCalculator</a></li>
      <li :class="{ active: isActive('/PasswordGenerator') }"><a href="#/PasswordGenerator" class="nav-item generator">PasswordGenerator</a></li>
      <li :class="{ active: isActive('/AccordionComponent') }"><a href="#/AccordionComponent" class="nav-item accordion">AccordionComponent</a></li>
      <li :class="{ active: isActive('/FormValidation') }"><a href="#/FormValidation" class="nav-item validation">FormValidation</a></li>
      <li :class="{ active: isActive('/ProgressSteps') }"><a href="#/ProgressSteps" class="nav-item progress">ProgressSteps</a></li>
      <li :class="{ active: isActive('/GithubUsersSearch') }"><a href="#/GithubUsersSearch" class="nav-item github">GithubUsersSearch</a></li>
      <li :class="{ active: isActive('/TodoList') }"><a href="#/TodoList" class="nav-item todo">TodoList</a></li>
      <li :class="{ active: isActive('/DadJokes') }"><a href="#/DadJokes" class="nav-item jokes">DadJokes</a></li>
      <li :class="{ active: isActive('/RandomQuoteGenerator') }"><a href="#/RandomQuoteGenerator" class="nav-item quote">RandomQuoteGenerator</a></li>
      <li :class="{ active: isActive('/WikiComponent') }"><a href="#/WikiComponent" class="nav-item wiki">WikiComponent</a></li>
    </ul>
  </nav>
  <component :is="currentView" />
</template>

 

<style scoped>
.navbar {
  padding: 5px;
  border-radius: 5px;
  text-align: center;
  margin-top: 2rem;
  margin-bottom: 2rem;
}

.navbar-nav {
  list-style-type: none;
  padding: 0;
  margin: 0;
  text-align: center; 
}

.navbar-nav li {
  display: inline-block;
  margin-right: 10px;
}

.navbar-nav li a {
  text-decoration: none;
  padding: 8px 12px;
  border-radius: 5px;
  transition: background-color 0.3s ease;
  color: #fff;  
}

.navbar-nav li a:hover {
  background-color: #555;
}

.home {
  background-color: #007bff;
}

.calculator {
  background-color: #28a745;
}

.generator {
  background-color: #ffc107;
}

.accordion {
  background-color: #17a2b8;
}

.validation {
  background-color: #dc3545;
}

.progress {
  background-color: #6f42c1;
}

.github {
  background-color: #e65c00;
}

.todo {
  background-color: #17a2b8;
}

.jokes {
  background-color: #ffc107;
}

.quote {
  background-color: #28a745;
}

.wiki {
  background-color: #007bff;
}

.active {
  background-color: #ffffff !important;  
}

.active a {
  background-color: #000000;
  color: #ffffff !important;  
}
.menu-icon {
  display: none;
  cursor: pointer;

}

.bar {
  width: 25px;
  height: 3px;
  margin-bottom: 4px;
  background-color: #000000;
  transition: background-color 0.3s ease;
}

.menu-icon:hover .bar {
  background-color: #ccc;
}

.navbar-nav {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.navbar-nav li {
  display: inline-block;
  margin-right: 10px;
}

.navbar-nav li a {
  text-decoration: none;
  padding: 10px 20px;
  border-radius: 5px;
  transition: background-color 0.3s ease;
  color: #fff;
  font-size: 16px;
  font-weight: bold;
}

.navbar-nav li a:hover {
  background-color: #555;
}

.active a {
  background-color: #000000e1;
}

.active a:hover {
  background-color: #777;
}

 
@media (max-width: 1690px) {
  .menu-icon {
    display: block;
  }
  
  .navbar-nav {
    display: none;
    position: absolute;
    top: 80px;
    width: auto;
    background-color: #444;
    padding: 10px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    z-index: 999;
    height: 100%;
 
  }
  
  .navbar-nav.active {
    display: block;
  }
 
  .navbar-nav li {
    display: block;
    margin: 10px 0;
  }
  
  .navbar-nav li a {
    display: block;
    padding: 10px 0;
    border-bottom: 1px solid #666;
  }
  .home {
  background-color: #007bff;
}

.calculator {
  background-color: #28a745;
}

.generator {
  background-color: #ffc107;
}

.accordion {
  background-color: #17a2b8;
}

.validation {
  background-color: #dc3545;
}

.progress {
  background-color: #6f42c1;
}

.github {
  background-color: #e65c00;
}

.todo {
  background-color: #17a2b8;
}

.jokes {
  background-color: #ffc107;
}

.quote {
  background-color: #28a745;
}

.wiki {
  background-color: #007bff;
}
}
 

</style>
