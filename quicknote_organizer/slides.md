---
theme: default
background: white
layout: none
class: text-center
highlighter: shiki
lineNumbers: false
info: QuickNote Organizer - A simple note-taking application
drawings:
  persist: false
transition: slide-left
title: QuickNote Organizer
---

<div class="quicknote-app">
  <MainContainer />
</div>

<script setup>
import MainContainer from './components/MainContainer.vue'
</script>

<style>
.quicknote-app {
  width: 100vw;
  height: 100vh;
  background-color: #f5f5f5;
  padding: 0;
  margin: 0;
}

body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}

:root {
  --slidev-theme-primary: #1976D2;
}
</style>
