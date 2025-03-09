<template>
  <section class="contacts-section">
    <table class="contacts-table">
      <thead>
        <tr>
          <th @click="sortBy('date')" class="sortable-header">
            Date
            <span class="sort-icon" v-if="sortColumn === 'date'">
              {{ sortDirection === 'asc' ? '▲' : '▼' }}
            </span>
          </th>
          <th @click="sortBy('name')" class="sortable-header">
            Name
            <span class="sort-icon" v-if="sortColumn === 'name'">
              {{ sortDirection === 'asc' ? '▲' : '▼' }}
            </span>
          </th>
          <th @click="sortBy('gender')" class="sortable-header">
            Gender
            <span class="sort-icon" v-if="sortColumn === 'gender'">
              {{ sortDirection === 'asc' ? '▲' : '▼' }}
            </span>
          </th>
          <th @click="sortBy('country')" class="sortable-header">
            Country
            <span class="sort-icon" v-if="sortColumn === 'country'">
              {{ sortDirection === 'asc' ? '▲' : '▼' }}
            </span>
          </th>
          <th @click="sortBy('email')" class="sortable-header email-header">
            Email
            <span class="sort-icon" v-if="sortColumn === 'email'">
              {{ sortDirection === 'asc' ? '▲' : '▼' }}
            </span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in sortedContacts" :key="contact.email" @click="showContactDetails(contact)"
          class="contact-row">
          <td class="light-font">{{ contact.date }}</td>
          <td class="bold-font">{{ contact.name }}</td>
          <td class="light-font">{{ contact.gender }}</td>
          <td>{{ contact.country }}</td>
          <td class="light-font email-cell">{{ contact.email }}</td>
        </tr>
      </tbody>
    </table>
    <!-- Contact Details Popup -->
    <div v-if="selectedContact" class="contact-popup">
      <div class="popup-content">
        <button class="close-btn" @click="closePopup">×</button>
        <h1>{{ selectedContact.name }}</h1>
        <div class="detail-row">
          <span class="detail-label">Date:</span>
          <span class="detail-value">{{ selectedContact.date }}</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">Status:</span>
          <span class="detail-value">Inactive</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">Gender:</span>
          <span class="detail-value">{{ selectedContact.gender }}</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">Country:</span>
          <span class="detail-value">{{ selectedContact.country }}</span>
        </div>
        <div class="detail-row">
          <span class="detail-label">Email:</span>
          <span class="detail-value">{{ selectedContact.email }}</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  contacts: {
    type: Array,
    required: true
  }
});

const selectedContact = ref(null);
const sortColumn = ref('name');
const sortDirection = ref('asc');

const sortedContacts = computed(() => {
  const sorted = [...props.contacts];

  sorted.sort((a, b) => {
    let valueA = a[sortColumn.value];
    let valueB = b[sortColumn.value];

    if (sortColumn.value === 'date') {
      valueA = new Date(valueA);
      valueB = new Date(valueB);
    }

    if (valueA < valueB) {
      return sortDirection.value === 'asc' ? -1 : 1;
    }
    if (valueA > valueB) {
      return sortDirection.value === 'asc' ? 1 : -1;
    }
    return 0;
  });

  return sorted;
});

const sortBy = (column) => {
  if (sortColumn.value === column) {
    sortDirection.value = sortDirection.value === 'asc' ? 'desc' : 'asc';
  } else {
    sortColumn.value = column;
    sortDirection.value = 'asc';
  }
};

const showContactDetails = (contact) => {
  selectedContact.value = contact;
};

const closePopup = () => {
  selectedContact.value = null;
};
</script>

<style scoped>
.contacts-section {
  padding: 2rem 15%;
  position: relative;
}

.contacts-table {
  width: 100%;
  border-collapse: collapse;
}

.contacts-table th {
  text-align: left;
  padding: 1rem;
  border-bottom: 1px solid #eee;
  color: #999;
  font-weight: normal;
}

.sortable-header {
  cursor: pointer;
  user-select: none;
  position: relative;
  transition: color 0.2s ease;
}

.sortable-header:hover {
  color: #3498db;
  background-color: #f5f9ff;
}

.sort-icon {
  display: inline-block;
  margin-left: 6px;
  color: #3498db;
  font-size: 0.8rem;
}

.contacts-table td {
  padding: 1.5rem;
  border-bottom: 1px solid #eee;
}

.contact-row {
  cursor: pointer;
  transition: all 0.2s ease;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin: 0.5rem 0;
}

.contact-row:hover {
  background-color: #f0f7ff;
  outline: 2px solid #3498db;
  outline-offset: -2px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.light-font {
  font-weight: lighter;
}

.bold-font {
  font-weight: bold;
}

.email-header {
  text-align: right !important;
}

.email-cell {
  text-align: right !important;
}

.contact-popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.popup-content {
  background-color: white;
  padding: 5rem;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  position: relative;
  width: 100%;
  max-width: 500px;
}

.close-btn {
  position: absolute;
  top: 10px;
  right: 15px;
  border: none;
  background: none;
  font-size: 24px;
  cursor: pointer;
  color: #999;
}

.close-btn:hover {
  color: #333;
}

.detail-row {
  margin-bottom: 0.8rem;
  display: flex;
}

.detail-label {
  font-weight: bold;
  min-width: 80px;
  color: #555;
  opacity: 0.5;
  font-weight: lighter;
}

.detail-value {
  color: #333;
}

h1 {
  margin-top: 0;
  padding-bottom: 0.8rem;
  margin-bottom: 1.2rem;
}

@media (max-width: 768px) {
  .contacts-table {
    display: block;
    overflow-x: auto;
  }

  .popup-content {
    width: 90%;
    margin: 0 1rem;
  }
}
</style>