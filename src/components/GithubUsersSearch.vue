<script setup>
import { ref } from "vue";

const username = ref("");
const userProfile = ref(null);
const error = ref(null);

const getUserProfile = async () => {
  try {
    const response = await fetch(
      `https://api.github.com/users/${username.value}`
    );
    const data = await response.json();

    if (response.ok) {
      userProfile.value = data;
      error.value = null;
    } else {
      userProfile.value = null;
      error.value = `Error: ${data.message}`;
    }
  } catch (err) {
    console.error("Error fetching data:", err);
    error.value = "An error occurred while fetching data.";
  }
};

</script>

<template>
  <div class="github-profile-viewer">
    <h1 class="app-title">GitHub User Profile Viewer</h1>

    <div class="input-container">
      <input
        v-model="username"
        placeholder="Enter GitHub username"
        @input="getUserProfile"
      />
    </div>

    <transition name="fade">
      <div v-if="userProfile" class="card user-profile">
        <div class="profile-header">
          <img :src="userProfile.avatar_url" :alt="userProfile.login" />
          <h2>{{ userProfile.name }}</h2>
          <p>{{ userProfile.bio }}</p>
        </div>
        <div class="profile-details">
          <div class="detail-item">
            <strong>Followers:</strong> {{ userProfile.followers }}
          </div>
          <div class="detail-item">
            <strong>Following:</strong> {{ userProfile.following }}
          </div>
          <div class="detail-item">
            <strong>Public Repos:</strong> {{ userProfile.public_repos }}
          </div>
          <div class="detail-item">
            <strong>Location:</strong> {{ userProfile.location }}
          </div>
        </div>
      </div>
    </transition>

    <transition name="fade">
      <div v-if="error" class="card error-message">
        <p>{{ error }}</p>
      </div>
    </transition>
  </div>
</template>

<style scoped>
.github-profile-viewer {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  text-align: center;
}

.app-title {
  font-size: 36px;
  color: #333;
  margin-bottom: 30px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

.input-container {
  margin-bottom: 20px;
}

input {
  width: 95%;
  padding: 12px;
  font-size: 18px;
  border: 2px solid #ccc;
  border-radius: 8px;
  transition: border-color 0.3s ease;
}

input:focus {
  border-color: #4caf50;
  outline: none;
}

.card {
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  overflow: hidden;
  margin-top: 20px;
  transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
}

.user-profile {
  display: flex;
  flex-direction: column;
}

.profile-header {
  padding: 20px;
}

img {
  width: 150px;
  height: 150px;
  object-fit: cover;
  border-radius: 50%;
  margin-bottom: 20px;
}

.profile-details {
  padding: 20px;
  text-align: left;
}

.detail-item {
  margin-bottom: 10px;
}

.error-message {
  color: #e74c3c;
  margin-top: 20px;
}
</style>
