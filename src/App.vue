<script setup>
import DashboardHeader from "./components/DashboardHeader.vue";
import UserProfile from "./components/UserProfile.vue";
import { ref } from 'vue';
const profile = ref({
  name: 'John Doe',
  lastOnline: '2 days ago',
  image: '/src/assets/Avatar.png'
});
</script>

<template>
    <DashboardHeader />
    <UserProfile :profile="profile" />
</template>

<style scoped>
</style>