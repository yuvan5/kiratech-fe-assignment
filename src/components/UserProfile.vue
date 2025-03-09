<template>
    <section class="profile-section">
        <div class="profile-container">
            <div class="profile-image-container">
                <img :src="profile.image" alt="Profile Picture" class="profile-image" />
            </div>
            <div class="profile-info">
                <div class="profile-details">
                    <h2 class="profile-name">{{ profile.name }}</h2>
                    <p class="profile-status">Last online: {{ profile.lastOnline }}</p>
                </div>
                <div class="profile-actions">
                    <button class="action-button message-button">
                        <i class="fa-brands fa-telegram"></i>
                        Send Message
                    </button>
                    <button class="action-button friend-button">
                        <i class="fa-solid fa-plus"></i>
                        Add Friend
                    </button>
                </div>
            </div>
        </div>
    </section>
    <div class="contacts-container">
        <div v-if="loading" class="loading">Loading contacts...</div>
        <div v-if="error" class="error">{{ error }}</div>
        <UserTable v-if="!loading && !error" :contacts="formattedContacts" />
    </div>
</template>

<script setup>
defineProps({
    profile: {
        type: Object,
        required: true
    }
});
import { ref, onMounted, computed } from 'vue';
import UserTable from './UserTable.vue';

const contacts = ref([]);
const loading = ref(true);
const error = ref(null);

// Format the api data for the table
const formattedContacts = computed(() => {
    return contacts.value.map(user => {
        const date = new Date(user.registered.date);
        const formattedDate = `${date.getDate()} ${date.toLocaleString('en-US', { month: 'short' })} ${date.getFullYear()}`
        const capitalizedGender = user.gender.charAt(0).toUpperCase() + user.gender.slice(1).toLowerCase();
        return {
            date: formattedDate,
            name: `${user.name.first} ${user.name.last}`,
            gender: capitalizedGender,
            country: user.location.country,
            email: user.email
        };
    });
});

const fetchContacts = async () => {
    loading.value = true;
    error.value = null;

    try {
        const response = await fetch('https://randomuser.me/api/?results=20');

        if (!response.ok) {
            throw new Error(`API response error: ${response.status}`);
        }

        const data = await response.json();
        contacts.value = data.results;
        loading.value = false;
    } catch (err) {
        error.value = `Failed to load contacts: ${err.message}`;
        loading.value = false;
        console.error('Error fetching contacts:', err);
    }
};

onMounted(() => {
    fetchContacts();
});
</script>

<style scoped>
.profile-section {
    background-color: #40c4dd;
    padding: 2rem;
    color: white;
    position: relative;
}

.profile-container {
    display: flex;
    align-items: center;
    gap: 2rem;
    max-width: 1000px;
    margin: 0 auto;
    position: relative;
}

.profile-info {
    position: relative;
    z-index: 1;
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 50%;
}

.profile-details {
    flex: 1;
}

.profile-image-container {
    width: 120px;
    height: 120px;
    border-radius: 8px;
    overflow: hidden;
    border: 3px solid white;
    transform: translateY(-20px);
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

.profile-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.profile-name {
    font-size: 2rem;
    margin: 0 0 0.5rem 0;
}

.profile-status {
    margin: 0 0 1rem 0;
    opacity: 0.9;
}

.profile-actions {
    display: flex;
    gap: 1rem;
}

.action-button {
    display: flex;
    align-items: center;
    padding: 0.6rem 1.2rem;
    border-radius: 4px;
    border: none;
    font-weight: bold;
    cursor: pointer;
    gap: 0.5rem;
}

.message-button {
    background-color: white;
    color: #40c4dd;
}

.friend-button {
    background-color: white;
    color: #40c4dd;
}

@media (max-width: 768px) {
    .profile-container {
        flex-direction: column;
        align-items: flex-start;
        align-items: center;
    }

    .profile-image-container {
        transform: translateY(0);
    }
}
</style>