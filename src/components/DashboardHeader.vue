<template>
    <header class="header">
      <div class="logo-container">
        <img src="@/assets/Logo.png" alt="KiraTech Logo" class="logo" />
      </div>
      <div class="header-actions">
        <button class="icon-button">
            <i class="fa-solid fa-bell"></i>
        </button>
        <button class="icon-button">
          <i class="fa-solid fa-gear"></i>
        </button>
        <button class="icon-button">
          <i class="fa-solid fa-right-from-bracket"></i>
        </button>
      </div>
    </header>
  </template>
  
  <script setup>
  </script>
  
  <style scoped>
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    background-color: white;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .logo-container {
    display: flex;
    align-items: center;
  }
  
  .logo {
    height: 40px;
    margin-right: 0.5rem;
  }
  
  .header-actions {
    display: flex;
    gap: 1rem;
  }
  
  .icon-button {
    background: none;
    border: none;
    cursor: pointer;
    padding: 0.5rem;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .icon-button:hover {
    background-color: rgba(0, 0, 0, 0.05);
  }
  </style>